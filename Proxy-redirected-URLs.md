Google proxies moved to https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#services-we-proxy-through-brave-servers.

Server config at https://github.com/brave/devops/wiki/Mappings-for-proxied-URLs.

## What requests to proxy?

If a service would cause network traffic to a non-Brave server in a new browser profile, prior to changing settings or interacting with specific features, then it should be proxied so that users don't talk to it directly (e.g. Chrome component updates, Safe Browsing).

## What requests **not** to proxy?

Services that provide data to the browser and are a result of a user choosing to interact with that feature/service do not typically need to be proxied (e.g. Chrome Store extensions, Wallet on-ramp, Uphold wallet linking).
