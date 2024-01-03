The Brave  query string filter aims at preventing the tracking of individual users without interfering with coarse-grained campaign-level tracking. We target parameters which are known to be specific to:
- a user,
- an email address, or
- an individual click.

This kind of tracking is typically used to sync cookie, that is the practice of synchronizing the value of first-party cookies on different domains, to link clicks inside an email message to a website visit, or to leak a user's identity across site boundaries.

In addition, we may remove parameters that can be used to circumvent our referrer trimming protections, that is parameters that would leak more than just the referring page's origin (e.g. including the referring page's path).

### Implementation

The way that the filter works is that we remove from the query string any parameters (i.e. the parameter name and its value) before we proceed with a non-same-site `GET` request (navigations, subresources and redirects). This means that such parameters never make it to the server, URL bar or the `Referer` header, and cannot be recovered by scripts running on a page.

A notable exception to this intervention is the unsubscribe link in emails. If a user-identifying tracking parameter is required for that functionality to work, we make an exception. For example, the `mkt_tok` parameter is removed except when the string `unsubscribe` is present in the URL.

All issues related to this feature are tagged with the [privacy/query-filter label](https://github.com/brave/brave-browser/issues?q=label%3Aprivacy%2Fquery-filter+).

### List

The current list of parameters we filter can be seen in the [`query_filter` component](https://github.com/brave/brave-core/blob/master/components/query_filter/utils.cc#L21).

There are three types of rules:

- simple: the parameter name is removed from any URL (case-sensitive)
- conditional: the parameter name is removed only if the given *regular expression* does **not** match the URL
- scoped: the parameter name is only removed if the URL's *base domain* matches one of the given domains

### QA

There is a test page at <https://fmarier.github.io/brave-testing/query-filter.html>.