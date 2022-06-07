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
* [ ] some images are missings

## electron18 enhancements status
* [ ] group1
  * [ ] components
  * [x] menu // notification fetching needs to be fixed, it's blocking menu creation
  * [ ] theming // broken, also broken in e11
* [ ] group2
  * [ ] font-chooser
  * [ ] tweaks
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
  * [ ] simpler-databases (gear icon) // seems to be broken, appears with e11 but buggy
  * [x] topbar-icons
  * [ ] truncated-titles // freezes the ui on hover, same with e11
  * [ ] weekly-view // seems to be broken, same with e11
  * [x] word-counter
* [ ] group5
  * [ ] icon-sets
  * [ ] quick-note
* [ ] group6: themes // broken, also broken in e11
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
