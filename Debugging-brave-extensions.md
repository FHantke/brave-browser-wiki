At the moment we have the following extensions:

* Shields (runs as browser action page -- named only `Brave`)
* Rewards (runs as browser action page)
* WebTorrent (runs as extension content page)
* Sync (background script only; `chrome://sync` UI is a built-in WebUI)

The visual parts of the extension could be debugged via usual way by clicking "Inspect" in the context menu. Note that for extension pages you will be debugging the content UI itself. There may also be scripts run in a separate long-running background context (see below).

To debug background scripts, go to `brave://inspect/#extensions` and select the extension you're looking to debug. A popup will open with background information. If the extension uses Redux, you should see the current store state.

Note that, at the time of this writing, you will need to do a build `npm run build` to see changes applied before debugging.