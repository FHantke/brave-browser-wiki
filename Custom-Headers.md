## Why use custom headers?

Brave has reached the size where we would like to add `brave` to the User-Agent string. However, in testing this, we found that many sites don't work correctly with this UA.

We added a JS API to [allow sites to detect brave](https://github.com/brave/brave-browser/issues/1052). Unfortunately this doesn't work for Brave's partners who require the ability to detect the UA in an HTTP header.

In all cases, the header is the same for **all users** of Brave Desktop and therefore cannot be used to distinguish users.

## X-Brave-Partner


| **Site**        | Header | Reason  |
| ----------------| -------| ------- |
| Brave           | `X-Brave-Access-Key: key` | [Show subscription page for Dow Jones Media Group](https://github.com/brave/brave-browser/issues/1805).
| cheddar.com     | `X-Brave-Partner: cheddar` | To provide [free subscriptions](https://brave.com/cheddar-partnership/) to Brave users.
| coinbase.com & subdomains | `X-Brave-Partner: coinbase` | For detection of using Brave for the [Earn BAT](https://brave.com/coinbase-earn-bat/) campaign.
| marketwatch.com & barrons.com | `X-Brave-Partner: dowjones` | To provide [free subscriptions](https://www.brave.com/dow-jones/) to Brave users.
| townsquare.com & related sites | `X-Brave-Partner: townsquare` |  [Offer to users](https://basicattentiontoken.org/townsquare-partnership) that are not in Brave to download Brave.
| softonic.[com\|cn\|jp\|pl\|com.br] | `X-Brave-Partner: softonic` | Avoid showing download Brave promotions to Brave users.
| upbit.com, upbitit.com, upbitit.be, upbitit.tv and subdomains | X-Brave-Partner: upbit | For detection of using Brave for co-marketing.
| uphold.com & subdomains | `X-Brave-Partner: uphold` | Shows different UI to Brave users for payments as a publisher.
| eaff.com & subdomains | `X-Brave-Partner: eaff` | Custom content for Brave users.
| grammarly.com & subdomains | `X-Brave-Partner: grammarly` | Attribution & Error Reporting

## Yandex CLID

If Yandex is the default search engine, we add a `clid` parameter to search requests to indicate that they are coming from Brave. Our `clid` is assigned by Yandex, so it looks like a random number but is actually the same for all users on a platform. On Android, you would see 2423859 in the URL and on desktop you would see 2353835.