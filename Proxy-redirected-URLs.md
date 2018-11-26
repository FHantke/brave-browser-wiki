To prevent direct connections to Google, Brave proxies some URLs through *.brave.com endpoints. Currently there are multiple Brave endpoints but eventually with https://github.com/brave/brave-browser/issues/2235, they will be combined into a single endpoint.

## Mappings

These obey Chromium's glob matching rules. https://developer.chrome.com/extensions/match_patterns

`<same<>` indicates that everything after the host in the URL is passed through to the new URL.

* `https://dl.google.com/release2/chrome_component/*crl-set*`: `https://crlsets1.brave.com/<same>`
* `https://*.gvt1.com/edgedl/release2/chrome_component/*crl-set*`: `https://crlsets2.brave.com/<same>`