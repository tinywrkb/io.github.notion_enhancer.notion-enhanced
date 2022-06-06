# TODO

* [ ] document/script: notion's package.json fetching & patching
* [ ] document/script: notion's package-lock.json generation
* [ ] document/script: generated-sources generation
* [ ] document/script: generation of redacted notion-package-lock.json
* [ ] document/script: replace homedir with app.getPath('appData') https://www.electronjs.org/docs/latest/api/app#appgetpathname
* [x] packaging: flatpak notion-enhancer module to /share/npm-modules + /bin
* [x] packaging: install node into /bin and as a separate module
* [x] packaging: rename electron binary
* [ ] packaging: consider installing empty app as a hack
* [x] packaging: update notion package.json & lock to install in flat folder
* [ ] packaging: use enhancer patches
* [x] packaging: more node_modules optimizations
* [ ] packaging: disable broken modules
* [ ] rename ./helpers/notionIpc to ./mainIpc

## bump electron and modules
* fixes tray icon
* fixes cjk fonts
* breaks most tweaks
* breaks multi-account login
* ref: [breaking changes](https://www.electronjs.org/docs/latest/breaking-changes)
* ref: [BrowserWindow](https://www.electronjs.org/docs/latest/api/browser-window)
* likely need `enableRemoteModule: true` in `webPreferences`

## possible future breakage
* context isolation switch to true
  * ref: https://www.electronjs.org/docs/latest/tutorial/context-isolation
  * example solution: use contextBridge
  * where? in electronApi.cjs, declaration of globalThis
* breaking changes @ https://www.electronjs.org/docs/latest/breaking-changes
* @electron.remote @ https://github.com/electron/remote

## electron tests: notion 2.0.27
* notion-enhancer: only supports e11
* multi-account
  * broken: e12?,e15,e19
  * works: e11,e13?,e14,e16,e17,18
* cjk: works from e14
* tray icon: works at least in e18

## electron18 generic issues
* [ ] some images are missings

## electron18 ext support
* [ ] base1
  * [ ] components
  * [x] menu // notification fetching needs to be fixed, it's blocking menu creation
  * [ ] theming // broken, also broken in e11
* [ ] base2
  * [ ] font-chooser
  * [ ] tweaks
* [ ] windowing
  * [ ] always-on-top // ? need to test in a DE
  * [ ] integrated-titlebar // broken? no titlebar in sway "electron.browser.on is not a function"
  * [ ] tabs // mostly working, hit an issue where all the window is gray out after creating a new tab, had bypass preview enabled
  * [ ] tray // working, need to test icon in flatpak
  * [x] view-scale
* [ ] somethingsoemthing
  * [x] bypass-preview
  * [x] calendar-scroll
  * [x] code-line-numbers
  * [x] collapsible-properties
  * [x] emoji-sets
  * [x] focus-mode
  * [x] global-block-links
  * [x] indentation-lines
  * [x] outliner
  * [x] right-to-left
  * [x] scroll-to-top
  * [ ] simpler-databases (gear icon) // seems to be broken, app in e11  but buggy
  * [x] topbar-icons
  * [ ] truncated-titles // freezes the ui on hover, also in e11
  * [ ] weekly-view // seems to be broken, also broken with e11
  * [x] word-counter
* [ ] somethingsoemthing2
  * [ ] icon-sets
  * [ ] quick-note
* [ ] themes // broken, also broken in e11
  * [ ] cherry-cola
  * [ ] dark+
  * [ ] dracula
  * [ ] gruvbox-dark
  * [ ] gruvbox-light
  * [ ] light+
  * [ ] material-ocean
  * [ ] neutral
  * [ ] nord
  * [ ] pastel-dark
  * [ ] pinky-boom
  * [ ] playful-purple
