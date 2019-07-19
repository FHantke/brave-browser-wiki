PageGraph Goal
---
PageGraph is a under development, research effort to instrument Brave, blink and v8, to allow for complete attribution of document modifications, network requests, script execution, and privacy-relevant Web API accesses.
It currently lives in the [`page-graph-integration`](https://github.com/brave/brave-core/tree/page-graph-integration/) branch of [`brave/brave-core`](https://github.com/brave/brave-core).  A test suite is maintained at [brave-experiments/page-graph-tests](https://github.com/brave-experiments/page-graph-tests).

PageGraph Currently Does the Following
---
* Tracking the execution and results of all script execution (inline, remote, attributes, eval, etc.)
* Tracking most resource requests (see below for exceptions, but currently covers AJAX, images, scripts and CSS), include response headers and size
* Track all DOM node creations, deletions and modifications (note small exceptions below)
* Track interactions with shields and network requests (including keeping track of which rules filter rules resulted in the behavior)
* Track when scripts call common Web API fingerprinting methods
* Puppeteer integration
* GraphML export for offline / after the fact analysis

Not Done But Will Be Done Shortly
---
* Time stamping all events

Would Be Nice / Someday / Known Limitations / Not Important for Current Needs
---
* Tracking large requests (`<audio>` and `<video>`)
* Dealing with WebSockets in anyway
* Better handling of JS urls (currently these are treated as originating from the document node, instead of the node with the relevant URL, b/c of how blink is structured)
* Tracking css `@imports`
* Tracking `style=` modifications
* Including request headers (currently only response headers are recorded)
* Better handling of HSTS and URL tracking. Currently requests are tracked w/o URL fragments (b/c they're ignored by the blink cache) or protocol (so that the same response can be matched with its request, even if it was HTST upgraded).  This works "well enough", but it's not 100% precise, and will loose information in some cases.
* Any kind of tracking of worker behaviors