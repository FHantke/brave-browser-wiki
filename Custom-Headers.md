Since Brave still has a relatively small user base, we avoid sharing extra information like a custom UA where possible.  We do have some exceptions though where we will share that a user is a Brave user through a custom header.


| **Site**        | Header | Reason  |
| ----------------| -------| ------- |
| Brave           | `X-Brave-Partner: uphold` |To present marketing information differently.
| cheddar.com     |        | To provide [free subscriptions](https://brave.com/cheddar-partnership/) to Brave users.
| coinbase.com    |        | For detection of using Brave for the [Earn BAT](https://brave.com/coinbase-earn-bat/) campaign.
| marketwatch.com & barrons.com | `X-Brave-Partner: dowjones` | To provide [free subscriptions](https://www.brave.com/dow-jones/) to Brave users.
| townsquare.com & related sites | `X-Brave-Partner: townsquare` |  [Offer to users](https://basicattentiontoken.org/townsquare-partnership) that are not in Brave to download Brave.
| uphold.com | | Shows different UI to brave users for payments as a publisher.