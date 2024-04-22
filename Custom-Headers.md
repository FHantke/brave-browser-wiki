## Why use Custom Headers or URL parameters?

Brave has reached the size where we would like to add `brave` to the User-Agent string. However, in testing this, we found that many sites don't work correctly with this UA.

We added a JS API to [allow sites to detect brave](https://github.com/brave/brave-browser/issues/1052). Unfortunately, this doesn't always work for Brave's partners. Sometimes the ability to detect the UA using a different mechanism is required.

In all cases, the information is the same for **all users** and therefore, cannot be used to distinguish users.

## Yandex CLID URL parameter

If Yandex is the default search engine, we add a `clid` parameter to search requests to indicate that they are coming from Brave. Our `clid` is assigned by Yandex, so it looks like a random number but is actually the same for all users on a platform. On Android, you would see `2423859` in the URL, and on desktop, you would see `2353835`.

## X-Brave-Ads-Enabled header

It helps the Brave Search backend to understand whether the requesting browser has Brave Rewards enabled. As such, the header `X-Brave-Ads-Enabled: 1` is added to navigation requests to Brave Search. Otherwise, the header is not added.