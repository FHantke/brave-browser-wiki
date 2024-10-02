This document describes the general structure of the PageGraph. The PageGraph is a graph stored in the GraphML format representing the DOM Tree of a web page, all its changes JavaScript makes to the DOM and all JavaScript activities that occur. It consists of directed edges and nodes, as explained in the following. Additionally, you can find some tips on how to understand the format.

The advantage of his format over other archiving formats is that it does not simply store the content of a page, but also all activities. That allows it to reproduce behavior from the time the page was visited. For example, a advertising or dynamic server exchanges that are miss-represented in classic formats like WARC or HAR can be reproduced with a PageGraph file.

This document is intended as a first introduction to the PageGraph and might be incomplete. Please verify the information before conducting your own experiments based on the PageGraph.

---
# A) Nodes
The graph consists of various nodes which we categorized into the following topics.
## Structure Nodes
Structure nodes represent all activities that relate to building the DOM.
### parser
Each document has its own parser node. If the parser belongs to the top page, it has no parent. It can also be the child of a *frame owner* node. Incoming edges can also come from resources if something is loaded in parallel with the document, e.g. using defer.
### DOM root
The DOM root indicates the root node of a document. It is the child node of a parser node connected via a *create node* edge. The node contains the URL the DOM belongs to. Its child node is the HTML element `<html>` that is connected via the *structure* edge. It follows HEAD, BODY, and the rest of the document structure as a DOM tree structure connected via *structure edge*s. All such HTML elements are inserted by the parser node. 

In the PageGraph, there is at least one DOM root for about:blank and one for the crawled top-level request.

This node has a *node id* attribute. It is used with the *insert node* edge to represent the structure of the DOM.
### Frame Owner
If there is an embedded element on the page, such as iframes or objects, it has the node type *frame owner* and the tag name corresponding to the tag element, e.g. IFRAME. This node always has a child node of the type *parser*.

This node has a *node id* attribute. It is used with the *insert node* edge to represent the structure of the DOM.
### HTML element
This node represents one HTML element inside a DOM. It could, for instance, be a div element or any other element (stored inside the tag attribute). It is always created by either a *parser* (if existing statically in the HTML) or a *script* node (if dynamically added via JavaScript). Additionally, HTML elements can cause the browser to start requests to load resources like images or scripts,  indicated by an outward *request start* edge.

If the tag is SCRIPT, the HTML node has an edge *create node* to a *script* node which contains all the script code.

This node has a *node id* attribute. It is used with the *insert node* edge to represent the structure of the DOM.
### text node
A text node is a structure node that holds the text value of HTML nodes. It has the *text* attribute containing the text value. It has a *structure* edge connecting it to the HTML element it belongs to and an *insert node* edge incoming from the parser or a script.

This node has a *node id* attribute. It is used with the *insert node* edge to represent the structure of the DOM.

## Storage Nodes
Storage nodes represent all activities related to the storage buckets, i.e., cookies, local storage, and sessions. If no storage bucket is actively used during the crawl on the page, the storage nodes build an independent graph in the structure.

Currently, cookies set via the HTTP responses are not represented in the PageGraph.
### storage
The storage node is the parent of all storage related nodes. It is connected with all storage buckets via *storage bucket* edges.
### Cookie Jar
The cookie jar is the node where all cookie-related activities are tracked.
### local storage
This node tracks all activities related to local storage.
### session storage
This node tracks all activities related to session storage.

## Java Script Nodes
Script nodes represent all activities related to JavaScript, its built-in calls and web APIs calls. Two of the nodes relate to actual JavaScript actions, together they make the group of JSStructureNodes: *web API* and *JS built-in*.
### script
Script nodes represent the scripts that are executed on a page. It contains the script code in the *source* attribute. These nodes represent various types of scripts, presented in the *script type* attribute. For example, if the script is an inline script it has the type "inline". It is then a child of a HTML script element it belongs to. The *script type* can also be "unknown" which means it could be an event handler or an extension. Scripts can do all kinds of action: they can create DOM elements; interact with storage buckets; create event listeners; call JSStructureNodes; create and execute other scripts.

The script node also has a *script id* attribute which is used for some edges like *set attribute* or *add event listener* to be related to a script.
### web API
A web API node is one of the JSStructureNodes and represents APIs like `navigator.getBattery()`. The node contains the attribute *method* with the exact method that was called and is connected via *js call* and *js result* edges to a script.
### JS built-in
A JS built-in node is one of the JSStructureNodes and represents built-in JS method like `Date.now()`. The node contains the attribute *method* with the exact method that was called and is connected via *js call* and *js result* edges to a script.

## Request Nodes
Request nodes represent all requests inside the page.
### resource
This node represents a resource that was requested by a HTML element or a script, for example an image or the JS file. It contains the requested URL in the *url*  attribute.

## Additional Nodes
### Extensions
Each parser node points to an extension node. At the moment, this node holds no information.

## Brave Related Nodes
The Brave Shields are also represented in the PageGraph. These nodes represent the structure of the BraveShields, yet they currently hold no information relevant for analysis. The Brave Shield is turned off by default when generating a PageGraph with the pagegraph-crawl tool.
### Brave Shields
The parent node of all shields is the Brave shield. Similar to the storage node, it connects all shields.
### Ads Shields
### FingerprintingV2 Shield
### javascript shield
### tracker shield
---
# B) Edges
All nodes are connected with directed edges. We use the same categories as for the nodes to categorize the edges.

## Structure Edges
Structure edges connect all structure nodes and present their activities.
### create node
Parser and script nodes can create other nodes. This edge represents this action by pointing from the parser or script node to the structure node that was created.
### insert node
This edge indicates that a structure node was not only created but then also inserted into a DOM. It has the attribute *parent* pointing to parent node id inside the DOM. It also has the *before* attribute which points to the element in the DOM that comes before this element.

Sometimes elements are created with JavaScript, but never added to the actual DOM of the document. By following the *insert node* edges, it is easy to understand what DOM the element belongs to.
### remove node
If a structure node is removed from the DOM, it is represented with this edge. The edge points from the parser or script node to the structure node that was removed.
### structure
This edge represents the structure inside the DOM. It points from the *DOM root*, *HTML element*, *parser*, or *script* nodes to other their child nodes. For example, the html element points to head and body, which then point with a structure edge to their own children.
### cross DOM
If the page contains multiple DOMs, for example due to an iframe, this DOM crossing is represented via this edge. The edge goes from the *frame owner* node to the corresponding new *parser* node.


## Attribute Edges
The attribute edges represent the action of setting or removing an attribute from an element.
### set attribute
This edge represents the setting of an attribute from a script or the parser. In its *key* attribute, it contains the attribute that was set; the value is stored in *value*.
### delete attribute
This edge represents the deletion of an attribute, containing the attribute name as *key*.

## Event Listener Edges
The PageGraph also records event listeners and their executions. The following edges represent various actions.
### event listener
This edge points to the script that is executed as the event handler. It goes from the structure element that has the event listener to a script with the script type "unknown". In the *key* attribute, this edge shows the corresponding listener.

The edge contains an *event listener id* attribute that is unique for every listener. It connects all actions belonging to this one listener.
### execute from attribute
This edge indicates that an event listener is presented inline as an attribute of an HTML element. It does not mean that this listener was executed. The edge points from the HTML element node to the belonging event handler script node, similar to the *event listener* edge.
### add event listener
This edge indicates that an event listener was added to a structure element. The edge goes either from a script or a parser node to a structure node. 

The edge contains a *event listener id* attribute that is unique for every listener. It connects all actions belonging to this one listener. This edge also contains a *script id* attribute that point to the last script that interacted with the event listener.
### remove event listener
This edge indicates that an event listener was removed from a structure element. The edge goes from a script node to the structure element node.

The edge contains a *event listener id* attribute that is unique for every listener. It connects all actions belonging to this one listener. This edge also contains a *script id* attribute that points to the last script that interacted with the event listener.

## Storage Edges
If a storage bucket is used on the page, the following edges are used to connect the storage bucket with the corresponding script node.
### storage set
This edge goes from a script node to one of the storage bucket nodes indicating the setting of a data items. The edge has a *key* and a *value* attribute.
### read storage call
This edge represents the read action for a storage interface by pointing from the script node to a storage bucket node. It has a *key* attribute to show what key is read. For cookies, this key is the origin.
### storage read result
This edge gives the result for the earlier read call and goes from the storage bucket node to the caller script node. It holds the *key* attribute that was requested and the corresponding *value*.
### delete storage
This edge indicates that a data item was deleted from a storage bucket. The edge goes from the script node to a storage bucket node. It has a *key* attribute indicating what key was deleted (even for none-existent keys).
### clear storage
This edge indicates that a storage bucket was cleared (e.g. `localStorage.clear()`). It goes from a script node to the storage bucket node.
### storage bucket
Finally, the last edge only connects all storage bucket nodes to the root *storage* node.

## JavaScript Edges
JavaScript executions are represented with three edges.
### js call
This edge points from a script node to JSStructure node which contains the method that was called. The edge contains the *script position* attribute indicating the position of the code that called the method in the *source* of the parent script node. The arguments of the call are stored in the *args* attribute.
### js result
As a result of a JS call, this edge indicates the successful execution and  points from the JSStructure node to the script node. The result returned is stored in the *value* attribute.
### execute 
This edge indicates who executed a script, for example an HTML element (script) or the parser itself. The edge points from the executor to the script.

## Request Edges
Request edges connect all request nodes. All request edges have a *request id* attribute belonging to the same request and a *resource type* attribute indicating the type of requested resource, e.g., Image or Script.
### request start
The *request start* edge goes from the element to a resource node indicating the started request.
### request error
If a request fails, this edges goes from the resource node to the initial requester node indicating an error.
### request redirect
If a request is redirected, this edge goes from the resource node that was requested to a new resource node representing the redirect. The initial resource node contains the initial *url*, the new node contains the redirect *url*.
### request complete
Finally, if the request was successful, this edge goes from the resource node back to the initial requester node containing the response headers in the attribute *headers* and the hash of the content and size of the response in the *response hash* and *size* attributes.

## Additional Edges
### shield
The shield edge connects all shield nodes.
