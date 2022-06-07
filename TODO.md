# TODO

* [ ] document/script: notion's package.json fetching & patching
* [ ] document/script: notion's package-lock.json generation
* [ ] document/script: generated-sources generation
* [ ] document/script: generation of redacted notion-package-lock.json
* [ ] document/script: replace homedir with app.getPath('appData') https://www.electronjs.org/docs/latest/api/app#appgetpathname
* [ ] packaging: disable broken modules
* [ ] review use of ./helpers/notionIpc and update to ./mainIpc

## possible future breakage and references
* [breaking changes](https://www.electronjs.org/docs/latest/breaking-changes)
* [@electron.remote](https://github.com/electron/remote)
* context isolation switch to true
  * [tutorial/context-isolation](https://www.electronjs.org/docs/latest/tutorial/context-isolation)
  * example solution: use contextBridge
  * where? in electronApi.cjs, declaration of globalThis
* [BrowserWindow](https://www.electronjs.org/docs/latest/api/browser-window)

## electron18 generic issues
* [ ] some page icons are missings

## electron18 enhancements status
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
