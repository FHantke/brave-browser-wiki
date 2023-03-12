The goal of the debouncer is to change server-side redirects into internal redirects so that redirecting servers don’t have the opportunity to set cookies or track an individual’s clicks. In other words, the destination URL after debouncing must be the same as if the user had visited the bounce tracker. For more information, see the [blog post](https://brave.com/privacy-updates/11-debouncing/) announcing this feature.

In particular, it’s not the job of the debouncer to:

- Upgrade URLs to HTTPS.
- Remove query parameters from the destination URL.

These other tasks are performed by other components in Brave.

Engineering issues: https://github.com/brave/brave-browser/issues?q=is%3Aissue+label%3Aprivacy%2Fdebounce+

Filter List issues: https://github.com/brave/adblock-lists/issues?q=is%3Aissue+label%3ADebounce+

# Security-related redirectors

Some redirectors, from well-known companies like Facebook and Google, are used to check outbound links for malicious URLs and content before letting users proceed. In addition, the extra confirmation page before leaving helps mitigate the threat of phishing attacks disguising the true origin of a landing page, e.g. `https://facebook.com/l.php?url=https://attacker.com/facebook-phishing.html`

As a result of a [Hacker One report](https://github.com/brave/brave-browser/issues/23026), we [removed](https://github.com/brave/adblock-lists/pull/862) some high-profile redirectors from our list. This kind of redirectors/bouncers are currently out-of-scope for the debouncer.

# Types of rules

At the moment, the following rule types are supported:

- `redirect`: the destination URL is extracted from a query string parameter
- `base64,redirect`: the destination URL is extracted from a query string parameter, but the value of the parameter is base64-encoded and must therefore be base64-decoded prior to redirecting to it
- `regex-path`: the destination URL is extracted from the URL path using a regex specified in the `param`

The `redirect` and `base64,redirect` rules expect the query string to be encoded in the standard way that HTML5 forms are (i.e. `?key1=value1&key2=value2`). It doesn’t work for any other encodings.

# Writing new rules

When writing new rules, these things must be determined:

1. Is it a security-related redirector? If so, don’t include.
2. Which query string parameter encodes the destination URL? (case **sensitive**)
3. Is the type of URL encoding supported?
4. What URL pattern should be used?


## Writing a regex rule (`regex-path` action)

The `regex-path` action lets you specify a generic regex pattern for picking out destination URLs from a given URL. The motivating use-case is to debounce AMP cache URLs to the canonical URLs. These URLs look something like https://www-theverge-com.cdn.ampproject.org/c/s/www.theverge.com/platform/amp/2018/9/20/17881766/bing-google-amp-support-mobile-news -- we would want to debounce that URL to https://www.theverge.com/platform/amp/2018/9/20/17881766/bing-google-amp-support-mobile-news. We also need the ability to predicate a debounce rule on a user preference, to support use-cases like De-AMP where a user turning off the preference should turn off the relevant debouncing rule.

The `param` should be a regular expression that has [one or more](https://github.com/brave/brave-core/pull/14687) regex capture groups that construct a well-formed URL on evaluation when applied on the original URL's path.

Note that all debounce rules are applied sequentially top-to-bottom, so higher rules take precedence. In other words, if you have two regex rules with the same `include` patterns, you should make sure the more specific one is higher in the list.

There's a key called `prepend_scheme: http|https` that, only if specified, will add the specified scheme (http or https) to the captured string value. Note that as a safety check, if the captured string value is already a valid URL AND `prepend_scheme` is specified, then we error out. `prepend_scheme` helps us capture the case in AMP cache URLs where the scheme is not specified in the original URL and would thus never be parsed into a valid URL.

Here's an example:

```
[
  {
    "include": [
      "*://brave.com/*"
    ],
    "pref": "brave.de_amp.enabled",
    "exclude": [
    ],
    "prepend_scheme": "https",
    "action": "regex-path",
    "param": "^/(.*)$"
  },
...
]
```

With this rule, the URL https://brave.com/braveattentiontoken.org would be debounced to https://braveattentiontoken.org/ if the user preference `brave.de_amp.enabled` (the De-AMP pref) is switched on. Note that https://brave.com/xyz would not be debounced despite matching the param pattern because xyz is not a valid URL even after prepending https scheme to it.

For more info, check the [GitHub issue](https://github.com/brave/brave-browser/issues/23121).

## Writing `redirect` and `base64,redirect` rules

### Identifying the right query string parameter

This part can be confusing for a few reasons.

Firstly, a query string can include both the **destination URL** and the **referring URL**:

```
https://shop-links.co/link/?publisher_slug=future&u1=tomsguide-us-9799394701951144000&exclusive=1&url=https%3A%2F%2Fwww.crutchfield.com%2FI-rNARc1BZH%2Fp_242ENDUROG%2FCleer-Enduro-ANC-Light-Grey.html&article_name=The%20best%20wireless%20headphones%20in%202021%20%7C%20Tom%27s%20Guide&article_url=https%3A%2F%2Fwww.tomsguide.com%2Fus%2Fbest-wireless-headphones%2Creview-5565.html 
```

In this case, `url` is the correct parameter to use, not `article_url`.

Secondly, some URLs can encode **multiple levels of redirections**:

```
https://clickserve.dartsearch.net/link/click?lid=12702340841222322&ds_s_kwgid=52302304781289121&ds_a_cid=125485223&ds_a_caid=9217121623&ds_a_agid=12532321223&ds_a_lid=kwd-15461251&&ds_e_adid=73312635129223&ds_e_target_id=kwd-73122551228234:loc-12&&ds_e_network=s&ds_url_v=2&ds_dest_url=https://t.myvisualiq.net/click_pixel?et=c&ago=212&ao=126&aca=71120122347799678&si=-2&ci=-2&pi=-2&ad=58120001282129221&sv1=43121240841212382&advt=-2&chnl=-2&vndr=335&sz=6583&u=6e790e6681711263e3af6fd3c9e2c788&red=https://www.bestbuy.com/site/electronics/top-deals/pcmcat1523299784494.c?id=pcmcat1563223712494&ref=213&loc=1&gclsrc=3p.ds&ds_rl=3223423&ref=212&loc=DWA
```

In this case, we find the destination URL in the `red` parameter, but following the original URL, we find that this is actually not the destination URL of the `clickserve.dartsearch.net` bouncer:

```
$ curl --head 'https://clickserve.dartsearch.net/link/click?lid=12702340841222322&ds_s_kwgid=52302304781289121&ds_a_cid=125485223&ds_a_caid=9217121623&ds_a_agid=12532321223&ds_a_lid=kwd-15461251&&ds_e_adid=73312635129223&ds_e_target_id=kwd-73122551228234:loc-12&&ds_e_network=s&ds_url_v=2&ds_dest_url=https://t.myvisualiq.net/click_pixel?et=c&ago=212&ao=126&aca=71120122347799678&si=-2&ci=-2&pi=-2&ad=58120001282129221&sv1=43121240841212382&advt=-2&chnl=-2&vndr=335&sz=6583&u=6e790e6681711263e3af6fd3c9e2c788&red=https://www.bestbuy.com/site/electronics/top-deals/pcmcat1523299784494.c?id=pcmcat1563223712494&ref=213&loc=1&gclsrc=3p.ds&ds_rl=3223423&ref=212&loc=DWA'
HTTP/2 302
...
location: https://t.myvisualiq.net/click_pixel?et=c&ago=212&ao=126&aca=71120122347799678&si=-2&ci=-2&pi=-2&ad=58120001282129221&sv1=43121240841212382&advt=-2&chnl=-2&vndr=335&sz=6583&u=6e790e6681711263e3af6fd3c9e2c788&red=https://www.bestbuy.com/site/electronics/top-deals/pcmcat1523299784494.c?id=pcmcat1563223712494&ref=213&loc=1&ds_rl=3223423&ref=212&loc=DWA
```

Instead, the first bouncer chains to a second bouncer, `t.myvisualiq.net`, and that second one is the one which makes use of the `red` parameter.

We are therefore looking at adding two separate rules since we **cannot skip a step in the normal redirect chain**:

- `ds_dest_url` on `clickserve.dartsearch.net`
- `red` on `t.myvisualiq.net`

Finally, in some cases, there is a URL parameter but we can’t use it because that’s **not the URL that the backend redirects to**:

```
https://bitdefender.f9tmep.net/c/338476/499160/4466?subtag=trd-nz-12659124619216761200&level=1&srcref=https%3A%2F%2Fwww.techradar.com%2F&brwsr=22221221-412c-21ec-b125-6fa12a2199af&brwsrsig=2Y1RI912PVraWUs12l2ePRiOx12U5V​​​​​
```

includes a `srcref` parameter that looks promising:

```
$ curl --head 'https://bitdefender.f9tmep.net/c/338476/499160/4466?subtag=trd-nz-12659124619216761200&level=1&srcref=https%3A%2F%2Fwww.techradar.com%2F&brwsr=22221221-412c-21ec-b125-6fa12a2199af&brwsrsig=2Y1RI912PVraWUs12l2ePRiOx12U5V​​​​​'
HTTP/2 301 
date: Wed, 08 Jun 2022 16:08:42 GMT
content-length: 0
location: https://ad.doubleclick.net/ddm/trackclk/N256806.2461108IMPACTRADIUSROW/B20028980.225381237;dc_trk_aid=423322651;dc_trk_cid=90273781;dc_lat=;dc_rdid=;tag_for_child_directed_treatment=;tfua=?clickid=&irgwc=1&MPid=338476&cid=aff%7Cc%7CIR
```

But the real destination URL is `ad.doubleclick.net` and not showing in the URL. Therefore this is **not a URL we can debounce**.

### Query-string encoding

In order to determine whether or not the encoding is supported:

1. Take the query string parameter which encodes the destination URL.
2. Follow it until the next `&` character.
3. Compare that extracted URL with the URL that the bouncer would redirect to.

For example, taking 
`https://clickserve.dartsearch.net/link/click?lid=12702340841222322&ds_s_kwgid=52302304781289121&ds_a_cid=125485223&ds_a_caid=9217121623&ds_a_agid=12532321223&ds_a_lid=kwd-15461251&&ds_e_adid=73312635129223&ds_e_target_id=kwd-73122551228234:loc-12&&ds_e_network=s&ds_url_v=2&ds_dest_url=https://t.myvisualiq.net/click_pixel?et=c&ago=212&ao=126&aca=71120122347799678&si=-2&ci=-2&pi=-2&ad=58120001282129221&sv1=43121240841212382&advt=-2&chnl=-2&vndr=335&sz=6583&u=6e790e6681711263e3af6fd3c9e2c788&red=https://www.bestbuy.com/site/electronics/top-deals/pcmcat1523299784494.c?id=pcmcat1563223712494&ref=213&loc=1&gclsrc=3p.ds&ds_rl=3223423&ref=212&loc=DWA`, we find that the `ds_dest_url` parameter is the one which contains the destination URL. Following that up to the next ampersand, we get a destination of `https://t.myvisualiq.net/click_pixel?et=c`. If we compare that with the redirect served by the server backend:

```
$ curl --head 'https://clickserve.dartsearch.net/link/click?lid=12702340841222322&ds_s_kwgid=52302304781289121&ds_a_cid=125485223&ds_a_caid=9217121623&ds_a_agid=12532321223&ds_a_lid=kwd-15461251&&ds_e_adid=73312635129223&ds_e_target_id=kwd-73122551228234:loc-12&&ds_e_network=s&ds_url_v=2&ds_dest_url=https://t.myvisualiq.net/click_pixel?et=c&ago=212&ao=126&aca=71120122347799678&si=-2&ci=-2&pi=-2&ad=58120001282129221&sv1=43121240841212382&advt=-2&chnl=-2&vndr=335&sz=6583&u=6e790e6681711263e3af6fd3c9e2c788&red=https://www.bestbuy.com/site/electronics/top-deals/pcmcat1523299784494.c?id=pcmcat1563223712494&ref=213&loc=1&gclsrc=3p.ds&ds_rl=3223423&ref=212&loc=DWA'
HTTP/2 302 
...
location: https://t.myvisualiq.net/click_pixel?et=c&ago=212&ao=126&aca=71120122347799678&si=-2&ci=-2&pi=-2&ad=58120001282129221&sv1=43121240841212382&advt=-2&chnl=-2&vndr=335&sz=6583&u=6e790e6681711263e3af6fd3c9e2c788&red=https://www.bestbuy.com/site/electronics/top-deals/pcmcat1523299784494.c?id=pcmcat1563223712494&ref=213&loc=1&ds_rl=3223423&ref=212&loc=DWA
```

we find that the URL we extracted is incomplete. Therefore this bouncer is **not supported by the debouncer**.

On the other hand, taking `https://go.redirectingat.com/?id=66960X1514304&xs=1&url=https://www.yubico.com/us/product/pivot2/&referrer=theverge.com&sref=https://www.theverge.com/2021/11/19/22791271/yubico-x-keyport-pivot-2-0-key-organizer-yubikey-security-key&xcust=___vg__p_22155112__m_social__s_twitter__t_w__c_the`, we extract the `url` parameter and following that to the next ampersand, we get `https://www.yubico.com/us/product/pivot2/`, which is the same as what the backend would redirect us to:

```
$ curl --head 'https://go.redirectingat.com/?id=66960X1514304&xs=1&url=https://www.yubico.com/us/product/pivot2/&referrer=theverge.com&sref=https://www.theverge.com/2021/11/19/22791271/yubico-x-keyport-pivot-2-0-key-organizer-yubikey-security-key&xcust=___vg__p_22155112__m_social__s_twitter__t_w__c_the'
HTTP/2 302 
...
location: https://www.yubico.com/us/product/pivot2/
```

and so this bouncer is **supported**.

### URL pattern

In order to avoid applying the client-side internal redirects to an overly-broad set of URLs, the matching URL pattern should be **as tight as possible**.

In general, this means:

- **No wildcard in the hostname part** of the URL unless this bouncer is known to use multiple sub-domains.
- The **path should be as specific as possible**, though it can include wildcards if needed.

Two other things to note:

- The query string is generally not as stable as a path and so **the match pattern should not include any parts of it** (e.g. the `shop-links.co` rule was [updated](https://github.com/brave/adblock-lists/pull/693) to include only the hostname and the path).
- All URL patterns should **begin and end with a wildcard** to catch any extra query string parameters a site might add, and to catch both the HTTP and HTTPS version of the page.

To summarize, a match pattern will generally look like `*://hostname.example.com/path/to/redirector/*`

# Testing new rules

Until a [built-in debugging interface](https://github.com/brave/brave-browser/issues/22996) is ready, the only way to test new rules is to edit the local debounce.json file:

- `/Users/<username>/Library/Application Support/BraveSoftware/Brave-Browser-Beta/afalakplffnnnlkncjhbmahjfjhmlkal/<version>/1/debounce.json` on Mac
- `/home/<username>/.config/BraveSoftware/Brave-Browser-Beta/afalakplffnnnlkncjhbmahjfjhmlkal/<version>/1/debounce.json` on Linux
- `/c/Users/.../AppData/Local/BraveSoftware/Brave-Browser-Nightly/User Data/afalakplffnnnlkncjhbmahjfjhmlkal/[version]/1/debounce.json` on Windows

and then restart the browser profile.