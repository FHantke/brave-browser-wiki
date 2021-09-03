# Wayback Machine Support in Brave

Brave version 1.4 introduced support (*enabled by default*) for [the Wayback Machine](https://blog.archive.org/2020/02/25/brave-browser-and-the-wayback-machine-working-together-to-help-make-the-web-more-useful-and-reliable/), enabling users to easily query for resources which are no longer available on the Web. The behavior of this feature is documented below, and the implementation can be viewed [on GitHub](https://github.com/brave/brave-core/tree/master/components/brave_wayback_machine).

## Overview

As a user browses the Web, some resources may be irretrievable (e.g. the resource has been deleted, the server or site is no longer online, etc.). When Brave detects a request for an unreachable resource, it will present the user with an option to *Check for saved version* via the Wayback Machine.

![Wayback Machine Support in Brave](https://user-images.githubusercontent.com/815158/132052142-e4d88c77-fec4-4e71-9547-ddb7f93455b8.png)

When a user chooses to query for earlier copies of the resource, Brave transmits the original resource URL to the Wayback Machine service for lookup (see *Resource Lookup* below). The user is informed whether copies of the requested resource exist, and can then decide if they would like to browse those copies.

Wayback Machine support can be controlled from within the Brave's Settings (`brave://settings/braveHelpTips/`).

## Observed Domain Patterns

Wayback Machine integration is supported for common URL patterns (e.g. http://\*.\*/, https://\*.\*/), with the exception of *localhost*, *\*.local*, and *\*.onion* TLDs, and any resource on the Wayback Machine domain itself (*web.archive.org*).

## Observed Status Codes

Before Brave will show the Wayback Machine infobar, it must first detect that the requested resource *is not* available. To determine availability, Brave evaluates the HTTP Response Code from the host server. The following codes will trigger the Wayback Machine infobar:

| Code | Description | Code | Description |
| ---- | ----------- | ---- | ----------- |
| 404  | HTTP Not Found | 509  | Bandwidth Limit Exceeded |
| 408  | HTTP Request Timeout | 520  | Server Returned an Unknown Error |
| 410  | HTTP Gone | 521 | Web Server is Down |
| 451  | Unavailable for Legal Reasons | 523 | Origin is Unreachable |
| 500  | Internal Server Error | 524 | A Timeout Occurred |
| 502  | Bad Gateway | 525 | SSL Handshake Failed |
| 503  | Gateway Timeout | 526 | Invalid SSL Certificate |

## Resource Lookup

When querying the Wayback Machine service for copies of a resource, Brave will issue a request to `brave-api.archive.org`. For example, if Brave was unable to reach `https://brave.com/`, and the user requested an earlier version, a request would be made for `https://brave-api.archive.org/wayback/available?url=https://brave.com/`. The requested resource/URL is passed to the Wayback Machine lookup service via the `?url` query-string parameter. The server response informs Brave whether a saved version of the resource exists or not.

The following is an example of a successful response:

```json
{
    "url": "https://brave.com/",
    "archived_snapshots": {
        "closest": {
            "status": "200",
            "available": true,
            "url": "http://web.archive.org/web/20210901235003/https://brave.com/",
            "timestamp": "20210901235003"
        }
    }
}
```

Requesting a resource that *does not* exist results in a response like the following:

```json
{
    "url": "https://example.site/",
    "archived_snapshots": {}
}
```

When this response is retrieved, it is parsed as JSON and and checked for a `archived_snapshots.closest.url` property. If the response cannot be parsed as JSON, or the resulting object lacks a URL, Brave will not show the infobar. However, if there are no issues parsing the content as JSON, and the resulting object contains a URL, Brave presents the infobar to the user.

## False Positives

HTTP Status Codes aren't always reliable on the Web. It's not uncommon to see a site serve the expected content with the wrong status code. It's also not uncommon to be redirected to a resource via an intermediary 404 status code. As a result, Brave may display the infobar at times even when the requested resource is available.

## Reporting Issues or Other Feedback

Please do not hesitate to reach out and share your thoughts and/or experience with Brave's integrated support for the Wayback Machine. Issues for Wayback Machine integration are filed with the [*feature/wayback machine* label](https://github.com/brave/brave-browser/labels/feature%2Fwayback%20machine) on GitHub. Feedback is also welcomed via [community.brave.com](https://community.brave.com/) and [Twitter](https://twitter.com/bravesupport/).