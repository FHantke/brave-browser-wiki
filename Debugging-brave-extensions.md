At the moment we have the following extensions:

* Shields (runs in browser action context -- named only `Brave`)
* Rewards (runs in browser action context)
* WebTorrent (runs in webUI context)
* Sync (runs in webUI context)

The visual parts of the extension could be debugged via usual way by clicking "Inspect" in the context menu. Note that for `webUI` extensions you will be debugging the `webUI` itself, which run scripts in the background (see below).

To debug background scripts, go to `brave://inspect/#extensions` and select the extension you're looking to debug. A popup will open with background information. If the extension uses Redux, you should see the current store state.

Note that, at the time of this writing, you will need to do a build `npm run build` to see changes applied before debugging.