## Why use Custom Headers?

Brave has reached the size where we would like to add `brave` to the User-Agent string. However, in testing this, we found that many sites don't work correctly with this UA.

We added a JS API to [allow sites to detect brave](https://github.com/brave/brave-browser/issues/1052). Unfortunately, this doesn't work for Brave's partners, who require the ability to detect the UA in an HTTP header.

In all cases, the header is the same for **all users** of Brave Desktop and, therefore, cannot be used to distinguish users.

## Desktop/Android

### X-Brave-Partner

The latest list used to be found at <https://laptop-updates.brave.com/promo/custom-headers>, but that has now moved to a static list in <https://github.com/brave/brave-core/blob/master/components/brave_referrals/browser/brave_referrals_service.cc>.

| **Site**        | Header | Reason  |
| ----------------| -------| ------- |
| grammarly.com & subdomain | `X-Brave-Partner: grammarly` | Count Brave visitors as part of an ad campaign

### Yandex CLID

If Yandex is the default search engine, we add a `clid` parameter to search requests to indicate that they are coming from Brave. Our `clid` is assigned by Yandex, so it looks like a random number but is actually the same for all users on a platform. On Android, you would see 2423859 in the URL, and on desktop, you would see 2353835.

### X-Brave-Ads-Enabled

It helps the Brave Search backend to understand whether the requesting browser has Brave Ads enabled. As such, the header `X-Brave-Ads-Enabled: 1` is added to navigation requests to Brave Search. Otherwise, the header is not added.

## iOS

### X-Brave-Ads-Enabled

It helps the Brave Search backend to understand whether the requesting browser has Brave Ads enabled. As such, the header `X-Brave-Ads-Enabled: 1` is added to navigation requests to Brave Search. Otherwise, the header is not added.
