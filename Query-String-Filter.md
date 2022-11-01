The Brave  query string filter aims at preventing the tracking of individual users without interfering with campaign-level tracking. We target parameters which are known to be specific to:
- a user,
- an email address, or
- an individual click.

This kind of tracking is typically used to sync cookie, that is the practice of synchronizing the value of first-party cookies on different domains, to link clicks inside an email message to a website visit, or to leak a user's identity across site boundaries.

### Implementation

The way that the filter works is that we remove from the query string any parameters (i.e. the parameter name and its value) before we proceed with a non-same-site `GET` request (navigations, subresources and redirects). This means that such parameters never make it to the server, URL bar or the `Referer` header, and cannot be recovered by scripts running on a page.

A notable exception to this intervention is the unsubscribe link in emails. If a user-identifying tracking parameter is required for that functionality to work, we make an exception. For example, the `mkt_tok` parameter is removed except when the string `unsubscribe` is present in the URL.

All issues related to this feature are tagged with the [privacy/query-filter label](https://github.com/brave/brave-browser/issues?q=label%3Aprivacy%2Fquery-filter+).

### List

The current list of parameters we filter can be seen in [brave/browser/net/brave_site_hacks_network_delegate_helper.cc:: kSimpleQueryStringTrackers](https://github.com/brave/brave-core/blob/master/browser/net/brave_site_hacks_network_delegate_helper.cc#L31).

### QA

There is a test page at <https://fmarier.github.io/brave-testing/query-filter.html>.