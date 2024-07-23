In order to measure Brave Ads performance, the Brave browser sends anonymous events to the server.

For non-Brave Rewards users (default state), the client will send a minimal set of information when the user interacts with Brave Ads. The exact information sent to the server is described in the [client README](https://github.com/brave/brave-core/blob/master/components/brave_ads/core/internal/account/confirmations/non_reward/README.md) and the server endpoint is `https://search.anonymous.ads.brave.com/v3/confirmation/{transactionId}`.

In addition:
* Our CDN uses the IP address to flag malicious traffic and identify the country.
* The [IP address is then stripped](https://github.com/brave/brave-browser/wiki/Stripping-IP-addresses#brave-ads) so that it's not accessible to our backend server.
* The user's platform (e.g. Windows, iOS) is derived from the User-Agent header.