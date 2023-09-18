Brave proxies some external services (not hosted by Brave) in order to hide users' IP addresses from these service providers.

The proxies for Google services are listed at https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#services-we-proxy-through-brave-servers.

Internal server configuration documentation can be found at https://github.com/brave/devops/wiki/Mappings-for-proxied-URLs (staff only)

# Guidelines for developers

Proxying has a privacy benefit for our users (hiding their IP address from third-parties), but it costs us in terms of maintenance and bandwidth. Also has a cost to our user in terms of latency. How do we pick which services to proxy?

## Requests that should be proxied

If a service would cause network traffic to a non-Brave server in a new browser profile, prior to changing settings or interacting with specific features, then it should be proxied so that users don't talk to it directly (e.g. Chrome component updates, Safe Browsing).

By default, the Brave browser should only talk to Brave servers.

## Requests that do NOT need be proxied

Services that provide data to the browser and are a result of a user choosing to interact with that feature/service do not typically need to be proxied (e.g. Chrome Store extensions, Wallet on-ramp, Uphold wallet linking).

When a user opts in and it's clear that the service is provided by a third-party, there is no need to proxy. It's similar to a user visiting Gmail because they chose Google as their email provider.