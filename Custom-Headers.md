## Why use Custom Headers?

Brave has reached the size where we would like to add `brave` to the User-Agent string. However, in testing this, we found that many sites don't work correctly with this UA.

We added a JS API to [allow sites to detect brave](https://github.com/brave/brave-browser/issues/1052). Unfortunately this doesn't work for Brave's partners who require the ability to detect the UA in an HTTP header.

In all cases, the header is the same for **all users** of Brave Desktop and therefore cannot be used to distinguish users.

## X-Brave-Partner


| **Site**        | Header | Reason  |
| ----------------| -------| ------- |
| eaff.com & subdomains | `X-Brave-Partner: eaff` | Custom content for Brave users.
| uphold.com & subdomains | `X-Brave-Partner: uphold` | Shows different UI to Brave users for payments as a publisher.

## Yandex CLID

If Yandex is the default search engine, we add a `clid` parameter to search requests to indicate that they are coming from Brave. Our `clid` is assigned by Yandex, so it looks like a random number but is actually the same for all users on a platform. On Android, you would see 2423859 in the URL and on desktop you would see 2353835.