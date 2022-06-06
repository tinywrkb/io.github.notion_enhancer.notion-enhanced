# manually building in a flatpak environment

## enter flatpak environment
```
flatpak run \
  --command=bash \
  --devel \
  --device=dri \
  --env=HOME=$PWD \
  --env=XDG_CACHE_HOME=$PWD/cache \
  --env=XDG_CONFIG_HOME=$PWD/config \
  --env=XDG_DATA_HOME=$PWD/data \
  --env=XDG_STATE_HOME=$PWD/state \
  --env=NPM_PACKAGES=$PWD/.npm-packages \
  --env=PATH=$PWD/.bin:$PWD/.npm-packages/bin:$HOME/projects/flatpak-dotuser/bin:/usr/bin:/usr/lib/sdk/node16/bin \
  --env=SSH_AUTH_SOCK=$XDG_RUNTIME_DIR/ssh-agent/sock \
  --env=npm_config_nodedir=/usr/lib/sdk/node16 \
  --filesystem=$PWD \
  --filesystem=xdg-config/ssh:ro \
  --filesystem=xdg-run/ssh-agent:ro \
  --filesystem=~/.user/hiddendots/openssh:ro \
  --filesystem=~/projects/flatpak-dotuser:ro \
  --share=ipc \
  --share=network \
  --socket=session-bus \
  --socket=system-bus \
  --socket=x11 \
  org.freedesktop.Sdk//21.08
```

## setup depends
```
install -dm755 .bin
npm config set prefix $PWD/.npm-packages

# go-yq
[ -e .bin/yq ] || {
  curl -L https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -o .bin/yq
  chmod +x .bin/yq
}

# 7za
[ -e .bin/7za ] || {
  npm install --global 7zip-bin
  ln -s ../.npm-packages/lib/node_modules/7zip-bin/linux/x64/7za .bin/7za
  chmod +x .bin/7za
}

# electron-builder
npm install --global electron-builder
```

## enhancer
```
# fetch sources
git clone --recurse-submodules https://github.com/notion-enhancer/desktop notion-enhancer

# build
(
  cd notion-enhancer
  npm install
  npm pack
  npm install --global notion-enhancer-*.tgz
)
```

## notion: set env vars
```
export NOTION_VER=2.0.27
export NOTION_REPACKAGED_REV=1
export NOTION_REPACKAGED_VER=${NOTION_VER}-${NOTION_REPACKAGED_REV}
```

## notion: download installer and extract sources
```
curl -L "https://desktop-release.notion-static.com/Notion%20Setup%20${NOTION_VER}.exe" -o notion_setup.exe
#7z e -so notion_setup.exe '$PLUGINSDIR/app-64.7z' > notion_app.7z
#bsdtar -xf notion_app.7z --exclude='resources/app/icon.*' --exclude=resources/app/node_modules --exclude='*.js.map' -s '#^resources/app#notion_src#' resources/app
7za x notion_setup.exe -x'!resources/app/icon.*' -x'!resources/app/node_modules' -xr'!*.js.map' resources/app && mv resources/app notion_src && rmdir resources
rm -f notion_app.7z notion_setup.exe
```

## notion: apply workarounds
```
pushd notion_src

sed -i 's|process.platform === "win32"|process.platform !== "darwin"|g' main/main.js
sed -i 's|sqliteServerEnabled: true|sqliteServerEnabled: false|g' renderer/preload.js
sed -i 's|error.message.indexOf("/opt/notion-app/app.asar") !== -1|process.platform === "linux"|g' main/autoUpdater.js

# avoid building better-sqlite3 from source
yq -i -p -o json \
  '.dependencies.better-sqlite3 = "7.4.5"' \
  package.json

# update cld
yq -i -P -o json \
  '.dependencies.cld="2.7.0"' \
  package.json

popd
```

## notion: update app details in package.json
```
(
  cd notion_src
  yq -i -P -o json \
    '.dependencies.cld="2.7.0" |
    .name="notion-app-enhanced" |
    .homepage="https://github.com/jamezrin/notion-repackaged" |
    .repository="https://github.com/jamezrin/notion-repackaged" |
    .author="Notion Repackaged" |
    .version=strenv(NOTION_REPACKAGED_VER)' \
    package.json
)
```

## notion: copy notion-enhanced icon
```
cp insert/media/colour-x512.png notion_src/icon.png
```

## notion: apply enhancements
```
pushd notion_src

# prepare
mkdir -p node_modules
ln -vs ./ app

# apply enhancements
notion-enhancer apply -y --no-backup --path=./

# cleanup
rm -vf app

# move notion-enhancer module to shared/
mv -v node_modules/notion-enhancer shared/
rmdir -v node_modules

# add notion-enhancer module to package.json
yq -i -P -o json \
  '.dependencies += {"notion-enhancer": "file:shared/notion-enhancer"}' \
  package.json

popd
```

## notion: build app
```
pushd notion_src

# install modules
npm install

# patch
npx patch-package

# install dev tools
npm install --save-dev electron@18 electron-builder

# package
electron-builder --config=../electron-builder.yaml

popd
)
```

## flatpak packaging update
```
NOTION_WRKDIR=
FLATPAK_SRCDIR=

# copy manifests and locks
cp ${NOTION_WRKDIR:-$PWD}/notion-enhancer/package.json ${FLATPAK_SRCDIR:-$PWD}/enhancer-package.json
cp ${NOTION_WRKDIR:-$PWD}/notion-enhancer/package-lock.json ${FLATPAK_SRCDIR:-$PWD}/enhancer-package-lock.json
cp ${NOTION_WRKDIR:-$PWD}/notion_src/package.json ${FLATPAK_SRCDIR:-$PWD}/notion-package.json
cp ${NOTION_WRKDIR:-$PWD}/notion_src/package-lock.json ${FLATPAK_SRCDIR:-$PWD}/notion-package-lock.json

# prepare node-package-lock.json for flatpak-node-generator
jq \
  'del(.dependencies."notion-intl") |
  del(.dependencies."notion-enhancer")' \
  notion-package-lock.json \
  > notion-package-lock-redacted.json

# generate sources
flatpak-node-generator.py npm --xdg-layout -o enhancer-sources.json enhancer-package-lock.json
flatpak-node-generator.py npm --xdg-layout -o notion-sources.json notion-package-lock-redacted.json

# cleanup
rm notion-package-lock-redacted.json
```
