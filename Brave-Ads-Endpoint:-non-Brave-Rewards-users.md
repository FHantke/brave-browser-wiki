In order to track Brave Ads metrics, the Brave browser sends confirmation events to the server.

For non-Brave Rewards users (default state), the client will send a private and reduced payload if the user has interacted with Brave Ads. The payload contains the properties as described in the [client README](https://github.com/brave/brave-core/blob/issues/38955/components/brave_ads/core/internal/account/confirmations/non_reward/README.md). The payload is sent to https://search.anonymous.ads.brave.com/v3/confirmation/{transactionId}

In addition:
* Our CDN uses the IP address to flag malicious traffic and identify the country.
* The [IP address is then stripped](https://github.com/brave/brave-browser/wiki/Stripping-IP-addresses#brave-ads) so that it's not accessible to our backend server.
* The user's platform (e.g. Windows, iOS) is derived from the User-Agent header.