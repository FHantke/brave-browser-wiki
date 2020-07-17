PageGraph Goal
---
PageGraph is an under-development, research effort to instrument Brave, blink and v8, to allow for complete attribution of document modifications, network requests, script execution, and privacy-relevant Web API accesses.
It currently lives in the [`page-graph`](https://github.com/brave/brave-core/tree/page-graph/) branch of [`brave/brave-core`](https://github.com/brave/brave-core).  A test suite is maintained at [brave-experiments/page-graph-tests](https://github.com/brave-experiments/page-graph-tests).

PageGraph's name comes from the graph-based representation of the document's execution it builds in memory.  Every relevant event in the document (a node being created, a network request being triggered, a script being executed, etc.) is recorded in the graph, noting both the relevant event, and the event's cause.

The resulting graph allows for the analysis of the proximate and upstream causes of every modification in the document. We expect this information will be useful for a variety of filter list generation, privacy-preserving feature restrictions, and web compatibility analysis, among other uses.

PageGraph is the next, production-ish version of [AdGraph](https://arxiv.org/abs/1805.09155), an earlier system for representing page execution as a directed graph.  PageGraph advances AdGraph in a large number of ways, increasing the breath and accuracy of attributions, enabling GraphML export, being kept up to date with Brave and Chromium, among many other improvements. 

If you have any questions about PageGraph, or would like to use / extend it for your research purposes, feel free to ping pes@brave.com / @pes10k

PageGraph Currently Does the Following
---
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

Not Done But Will Be Done Shortly
---
* Scripts in SVG documents

Would Be Nice / Someday / Known Limitations / Not Important for Current Needs
---
* Tracking large, streamed, requests (`<audio>` and `<video>`)
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
```
git clone -b page-graph https://github.com/brave/brave-browser
cd brave-browser
npm install
npm run init
npm run build -- Static
 ```

# PageGraph Documentation
## Types created by PageGraph
C++ definition locations below are relative to the `src/brave/third_party_blink_brave_page_graph/graph_item` directory of the `brave-core` repository.
### Node types

| Node type                 | C++ Definition                          | Attributes                                 |
|---------------------------|-----------------------------------------|--------------------------------------------|
| `"extensions"`            | node_extensions.cc                      |                                            |
| `"remote frame"`          | node_remote_frame.cc                    | `url`                                      |
| `"resource"`              | node_resource.cc                        | `url`                                      |
| `"ad filter"`             | filter/node_ad_filter.cc                | `rule`                                     |
| `"tracker filter"`        | filter/node_tracker_filter.cc           | ??                                         |
| `"fingerprinting filter"` | filter/node_fingerprinting_filter.cc    | ??                                         |
| `"web API"`               | js/node_js_webapi.cc                    | `method`                                   |
| `"JS builtin"`            | js/node_js_builtin.cc                   | `method`                                   |
| `"HTML element"`          | html/node_html_element.cc               | `tag name`, `is deleted`, `node id`        |
| `"text node"`             | html/node_html_text.cc                  | `text`, `is deleted`, `node id`            |
| `"DOM root"`              | html/node_dom_root.cc                   | `url`, `tag name`, `is deleted`, `node id` |
| `"frame owner"`           | html/node_frame_owner.cc                | `tag name`, `is deleted`, `node id`        |
| `"storage"`               | storage/node_storage_root.cc            |                                            |
| `"local storage"`         | storage/node_storage_localstorage.cc    |                                            |
| `"session storage"`       | storage/node_storage_sessionstorage.cc  |                                            |
| `"cookie jar"`            | storage/node_storage_cookiejar.cc       |                                            |
| `"script"`                | actor/node_script.cc                    | `url`, `script type`, `script id`          |
| `"parser"`                | actor/node_parser.cc                    |                                            |
| `type_ + " shield"`       | shield/node_shield.cc                   |                                            |
| `"Brave Shields"`         | shield/node_shields.cc                  |                                            |

### Edge types

| Edge Type                  | C++ Definition                               | Attributes                                                        |
|----------------------------|----------------------------------------------|-------------------------------------------------------------------|
| `"filter"`                 | edge_filter.cc                               |                                                                   |
| `"structure"`              | edge_html.cc                                 |                                                                   |
| `"cross DOM"`              | edge_cross_dom.cc                            |                                                                   |
| `"resource block"`         | edge_resource_block.cc                       |                                                                   |
| `"shield"`                 | edge_shield.cc                               |                                                                   |
| `"text change"`            | edge_text_change.cc                          |                                                                   |
| `"remove node"`            | node/edge_node_remove.cc                     |                                                                   |
| `"delete node"`            | node/edge_node_delete.cc                     |                                                                   |
| `"insert node"`            | node/edge_node_insert.cc                     | `parent`, `before`                                                |
| `"create node"`            | node/edge_node_create.cc                     |                                                                   |
| `"js result"`              | js/edge_js_result.cc                         | `value`                                                           |
| `"js call"`                | js/edge_js_call.cc                           | `args`                                                            |
| `"request complete"`       | request/edge_request_complete.cc             | `resource type`, `status`, `value`, `response hash`, `request id` |
| `"request error"`          | request/edge_request_error.cc                | `status`, `request id`, `value`                                   |
| `"request start"`          | request/edge_request_start.cc                | `request type`, `status`, `request id`                            |
| `"request response"`       | request/edge_request_response.cc             | ??                                                                |
| `"add event listener"`     | event_listener/edge_event_listener_add.cc    | `key`, `event listener id`, `script id`                           |
| `"remove event listener"`  | event_listener/edge_event_listener_remove.cc | `key`, `event listener id`, `script id`                           |
| `"event listener"`         | event_listener/edge_event_listener.cc        | `key`, `event listener id`,                                       |
| `"storage set"`            | storage/edge_storage_set.cc                  | `key`, `value`                                                    |
| `"storage read result"`    | storage/edge_storage_read_result.cc          | `key`, `value`                                                    |
| `"delete storage"`         | storage/edge_storage_delete.cc               | `key`                                                             |
| `"read storage call"`      | storage/edge_storage_read_call.cc            | `key`                                                             |
| `"clear storage"`          | storage/edge_storage_clear.cc                | ??                                                                |
| `"storage bucket"`         | storage/edge_storage_bucket.cc               |                                                                   |
| `"execute from attribute"` | execute/edge_execute_attr.cc                 | `attr name`                                                       |
| `"execute"`                | execute/edge_execute.cc                      |                                                                   |
| `"set attribute"`          | attribute/edge_attribute_set.cc              | `key`, `value`, `is style`                                        |
| `"delete attribute"`       | attribute/edge_attribute_delete.cc           | `key`, `is style`                                                 |