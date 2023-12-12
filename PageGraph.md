PageGraph is a research project developed by Brave to instrument Brave browser, blink and v8, to allow for complete attribution of document modifications, network requests, script execution, and privacy-relevant Web API accesses.

PageGraph is included in all builds of Brave starting with version 1.46, though enabling it requires passing some command line arguments. The easiest way to run PageGraph is to use [`pagegraph-crawl`](https://github.com/brave-experiments/pagegraph-crawl/) tools and scripts, which automate enabling PageGraph in Brave, launching Brave, and recording the resulting graph files.

*Note* that recording builtin JS APIs requires [nightly Brave builds](https://brave.com/download-nightly/). This choice is made to slightly optimize the performance of the Beta and Stable builds that most Brave users use. A list of which Web APIs are instrumented can be found in [python interface.py](https://github.com/brave/brave-core/blob/master/chromium_src/third_party/blink/renderer/bindings/scripts/bind_gen/interface.py#L32) file which controls, at build time, which APIs are instrumented. You can instrument additional APIs by indicating additional APIs in this file, and rebuilding Brave.

PageGraph's name comes from the graph-based representation of the document's execution it builds in memory.  Every relevant event in the document (a node being created, a network request being triggered, a script being executed, etc.) is recorded in the graph, noting both the relevant event, and the event's cause.

The resulting graph allows for the analysis of the proximate and upstream causes of every modification in the document. We expect this information will be useful for a variety of filter list generation, privacy-preserving feature restrictions, and web compatibility analysis, among other uses.

PageGraph is the next, production-ish version of [AdGraph](https://arxiv.org/abs/1805.09155), an earlier system for representing page execution as a directed graph.  PageGraph advances AdGraph in a large number of ways, increasing the breath and accuracy of attributions, enabling GraphML export, being kept up to date with Brave and Chromium, among many other improvements. 

Questions / Clarifications
---
If you have any questions about PageGraph, or would like to use / extend it for your research purposes, feel free to ping pes@brave.com / [@pes10k](https://twitter.com/pes10k).

## Features 

### PageGraph Currently Does the Following
* Tracking the execution and results of all script execution (inline, remote, attributes, eval, etc.) and how the script got on the page.
* Tracking most resource requests (see below for exceptions, but currently covers AJAX, fetch, images, scripts and CSS), include response headers and size
* Track all DOM node creations, deletions and modifications (note small exceptions below)
* Track interactions with shields and network requests (including keeping track of which rules filter rules resulted in the behavior)
* Track when scripts call common Web API fingerprinting methods
* Puppeteer integration
* GraphML export for offline / after the fact analysis
* Chromium devtools integration, for in-browser analysis of the graph.  Currently allows for observing the full history of any node in the document.
* Timestamp all events
* Module scripts
* Handling remote frames
* Instrumenting what parts of JS are responsible for which API calls

### Not Done But Will Be Done Shortly
* Scripts in SVG documents

### Would Be Nice / Someday / Known Limitations / Not Important for Current Needs
* Recording the body of large, streamed, requests (`<audio>` and `<video>`)
* Dealing with WebSockets in anyway
* Better handling of JS urls (currently these are treated as originating from the document node, instead of the node with the relevant URL, b/c of how blink is structured)
* Tracking css `@imports`
* Tracking `style=` modifications
* Including request headers (currently only response headers are recorded)
* Better handling of HSTS and URL tracking. Currently requests are tracked w/o URL fragments (b/c they're ignored by the blink cache) or protocol (so that the same response can be matched with its request, even if it was HTST upgraded).  This works "well enough", but it's not 100% precise, and will loose information in some cases.
* Any kind of tracking of worker behaviors
* Resources fetched b/c of CSS rules are currently tracked in the graph, but not attributed to either the relevant CSS document / rule, or the DOM element that triggered the request.  Would be nice to tie the request to either or both.

Building
---
As of version 1.46, PageGraph is included in all brave-core (i.e., desktop and Android) versions of Brave.  There is no need for a special, customized build process.

Running
---
The PageGraph-enabled browser can be run manually like a normal build of Brave, and can still be used to generate graph files when run manually. However it is recommended to run using an automated crawling tool like [pagegraph-crawl](https://github.com/brave-experiments/pagegraph-crawl).

## Publications Using PageGraph

- [Blocked Or Broken? Automatically Detecting When Privacy Interventions Break Websites](https://arxiv.org/abs/2203.03528), Michael Smith, Peter Snyder, Moritz Haller, Ben Livshits, Hamed Haddadi, Deian Stefan, **PETS 2022**
- [Measuring the Privacy vs. Compatibility Trade-off in Preventing Third-Party Stateful Tracking](https://www.peteresnyder.com/static/papers/storage-policies-www-2022.pdf), Jordan Jueckstock, Peter Snyder, Shaown Sarker, Alexandros Kapravelos, Ben Livshits, **WWW 2022**
- [SugarCoat: Programmatically Generating Privacy-Preserving, Web-Compatible Resource Replacements For Content Blocking](https://www.peteresnyder.com/static/papers/sugarcoat-ccs-2021.pdf), Michael Smith, Peter Snyder, Ben Livshits, Deian Stefan, **CCS 2021**
- [Detecting Filter List Evasion With Event-Loop-Turn Granularity JavaScript Signatures](https://arxiv.org/abs/2005.11910), Quan Chen, Peter Snyder, Benjamin Livshits, Alexandros Kapravelos, **S&P 2021**
- [Hiding in Plain Site: Detecting JavaScript Obfuscation through Concealed Browser API Usage](https://kapravelos.com/publications/jsobf-imc20.pdf), Shaown Sarker, Jordan Jueckstock, Alexandros Kapravelos, **IMC 2020**
- [Filter List Generation for Underserved Regions](https://arxiv.org/abs/1910.07303), Alexander Sjosten, Peter Snyder, Antonio Pastor, Panagiotis Papadopoulos, Benjamin Livshits, **WWW 2020**
- [Keeping Out the Masses: Understanding the Popularity and Implications of Internet Paywalls](https://arxiv.org/abs/1903.01406), Panagiotis Papadopoulos, Peter Snyder, Benjamin Livshits, **WWW 2020**


## PageGraph Documentation

The best place to find documentation on the structure and types in the resulting [GraphML](http://graphml.graphdrawing.org/)-format graphs is in the documentation for the [rust library](https://docs.rs/pagegraph) we maintain for parsing and querying these graphs. In particular, the [types documentation](https://docs.rs/pagegraph/0.1.3/pagegraph/types/index.html#types) may be helpful. 

This documentation is incomplete and being built up as we go. If you have questions about the document format that aren't answered by the existing documentation, please do not hesitate to [open an issue](https://github.com/brave/pagegraph-rust).