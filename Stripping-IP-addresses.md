# Brave Ads

In order to prevent the Brave Ads backend application from having access to
the user's IP address, the [anonymous confirmation endpoints](https://github.com/brave/brave-browser/wiki/Brave-Ads-Endpoint:-non-Brave-Rewards-users)
strips our users' IP address at the CDN layer.

Since these endpoints are hosted on [Fastly](https://www.fastly.com/)'s infrastructure, we use the
[`header.filter()` VCL
function](https://www.fastly.com/documentation/reference/vcl/functions/headers/header-filter/)
with the following list of headers to remove:

- `cf-connecting-ip`
- `cf-pseudo-ipv4`
- `cloudfront-viewer-address`
- `fastly-client-ip`
- `fastly-temp-xff`
- `forwarded`
- `forwarded-for`
- `true-client-ip`
- `x-appengine-user-ip`
- `x-client-ip`
- `x-cluster-client-ip`
- `x-forwarded`
- `x-forwarded-for`
- `x-real-ip`

In addition, the backend application contains middleware which looks for the
presence of these headers and rejects any request which includes them. This
fail-safe allows us to easily detect any failure or misconfiguration of the
CDN layer.

This is the middleware code which performs this check:
```
export function rejectIdentifyingHeaderMiddleware(
  req: Request,
  res: Response,
  next: NextFunction,
) {
  Object.entries(req.headers).forEach(([key, value]) => {
    if (!IP_HEADERS.has(key)) return;
    if (_.isEmpty(value)) return;
    logger.error("request contained identifying header", { name: key });
    throw new ServiceUnavailableException();
  });

  next();
}
```
where `IP_HEADERS` contains the list of headers shown above.

# Brave News

Proxied images in Brave News [use a private CDN](https://brave.com/blog/brave-private-cdn/) which prevents Brave from associating IP addresses with specific image requests.

# Private Advertising Analytics

For [`p2a.brave.com`](https://github.com/brave/brave-browser/wiki/Randomized-Response-for-Private-Advertising-Analytics), we use a [reverse proxy operated by Cloudflare](https://developers.cloudflare.com/spectrum/) to prevent IP addresses from reaching our backend server.