# Notion Enhanced Flatpak

Here you can find Flatpak packaging for [Notion Enhanced](https://notion-enhancer.github.io/) to easily build and
install the Electron Notion application with notion-enhancer.  
The Flatpak app differs from the pre-built Notion Enhanced packages by including more recent releases of Notion and Electron.  
This is a temporary solution, as upstream intend to submit the app to Flathub.org.

## How to build and install
* Clone this repository: `$ git clone https://github.com/tinywrkb/io.github.notion_enhancer.notion-enhanced.git`
* Build into a Flatpak repo, and install with Flatpak Builder
```
flatpak-builder \
  --install \
  --user \
  --force-clean \
  --state-dir=flatpak-builder \
  --repo=flatpak-repo \
  flatpak-target \
  io.github.notion_enhancer.notion-enhanced.yaml
```

## Enhancements status
* [ ] group1
  * [ ] components // untested
  * [x] menu // notification fetching needs to be fixed, it's blocking menu creation
  * [x] theming
* [ ] group2
  * [ ] font-chooser // untested
  * [ ] tweaks // untested
* [ ] group3: windowing
  * [ ] always-on-top // ? need to test in a DE
  * [ ] integrated-titlebar // broken? need to test in a DE, no titlebar in sway "electron.browser.on is not a function"
  * [x] tabs // mostly working, hit an issue where all the window is gray out after creating a new tab (bypass preview was enabled)
  * [x] tray
  * [x] view-scale
* [ ] group4
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
  * [ ] simpler-databases (gear icon) // broken, not e18 specific, see https://github.com/notion-enhancer/repo/issues/64
  * [x] topbar-icons
  * [ ] truncated-titles // freezes the ui on hover, same with e11
  * [ ] weekly-view // broken, not e18 specific, see https://github.com/notion-enhancer/repo/issues/112
  * [x] word-counter
* [ ] group5
  * [ ] icon-sets // untested
  * [ ] quick-note // untested
* [x] group6: themes // require changing the appearance setting to match the theme dark/light, see https://github.com/notion-enhancer/desktop/issues/723
  * [x] cherry-cola
  * [x] dark+
  * [x] dracula
  * [x] gruvbox-dark
  * [x] gruvbox-light
  * [x] light+
  * [x] material-ocean
  * [x] neutral
  * [x] nord
  * [x] pastel-dark
  * [x] pinky-boom
  * [x] playful-purple

## Other issues
* [ ] Some page's icons are missings. Electron 18 specific issue, and likely will be fixed by an update to Notion.

## TODO
* [ ] Replace homedir with app.getPath('appData'). [Reference](https://www.electronjs.org/docs/latest/api/app#appgetpathname)
* [ ] Disable broken enhancements
* [ ] Disable auto-update code
* [ ] Add documentation/script updates

## Possible future breakage and references
* [Breaking changes](https://www.electronjs.org/docs/latest/breaking-changes)
* [@electron.remote](https://github.com/electron/remote)
* context isolation switch to true
  * [tutorial/context-isolation](https://www.electronjs.org/docs/latest/tutorial/context-isolation)
  * example solution: use contextBridge
  * where? electronApi.cjs, declaration of globalThis
* [BrowserWindow](https://www.electronjs.org/docs/latest/api/browser-window)
* `./helpers/notionIpc` changed to `./mainIpc`
