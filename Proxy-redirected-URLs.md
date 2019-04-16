To prevent direct connections to Google, Brave proxies some URLs through *.brave.com endpoints. Currently there are multiple Brave endpoints but eventually with https://github.com/brave/brave-browser/issues/2235, they will be combined into a single endpoint.

## Mappings

These obey Chromium's glob matching rules. https://developer.chrome.com/extensions/match_patterns

`<same<>` indicates that everything after the host in the URL is passed through to the new URL.

* `https://dl.google.com/release2/chrome_component/*crl-set*`: `https://crlsets1.brave.com/<same>`
* `https://*.gvt1.com/edgedl/release2/chrome_component/*crl-set*`: `https://crlsets2.brave.com/<same>`
* `https://safebrowsing.googleapis.com/`: `https://safebrowsing.brave.com`
* `https://ssl.gstatic.com`: `https://static.brave.com`
* `https://gstatic.com`: `https://static1.brave.com`
* `https://update.googleapis.com`: `https://componentupdater.brave.com`
* `https://clients2.googleusercontent.com`: `https://crxdownload.brave.com`
- Page translation requests
  * `https://translate.googleapis.com/translate_a/element.js*`: `https://translate.brave.com/<same>`
  * `https://translate.googleapis.com/element/*/js/element/element_main.js`: `https://translate.brave.com/<same>`
  * `https://translate.googleapis.com/translate_static/js/element/main.js`: `https://translate.brave.com/<same>`
  * `https://translate.googleapis.com/translate_static/css/translateelement.css`: `https://translate.brave.com/<same>`
  * `https://www.gstatic.com/images/branding/product/*x/translate_24dp.png`: `https://translate-static.brave.com/<same>`