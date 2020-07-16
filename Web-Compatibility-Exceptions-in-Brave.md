This is a list of URL patterns that we have had to intentionally exclude from tracking protection due to site features breaking and user complaints. Note that these sites are not allowed access to third party cookies/storage.

We're in the process of adding options to control these exceptions as outlined in the "Summary of the plan" section of [this issue](https://github.com/brave/brave-browser/issues/3475).

## Filter lists

### Facebook login buttons

Related issues: https://github.com/brave/browser-laptop/issues/780, https://github.com/brave/browser-laptop/issues/880, https://bugzilla.mozilla.org/show_bug.cgi?id=1226498, https://github.com/brave/browser-laptop/issues/2014

Notes: We may be able to fix some of these issues by loading FB's SDK from a local file or polyfill instead of calling out to their domains. The SDK is often used for "Login with Facebook" buttons.

```
! Fully block Facebook everywhere but unbreak logins like from sites like quora.com and twitch.tv
! Note that options will be added to exclude these filters soon.
||facebook.com$third-party
||facebook.net$third-party
||staticxx.facebook.com$third-party
@@||connect.facebook.com/*/sdk.js$script
@@||connect.facebook.net/*/sdk.js$script
@@||facebook.com/connect/
@@||www.facebook.com/connect
@@||staticxx.facebook.com/connect/
@@||graph.facebook.com/
```

### Facebook embeds

```
! Block fbcdn.net everywhere but allow Facebook embeds
||fbcdn.net$third-party,domain=~facebook.com
@@||staticxx.facebook.com/
@@||xx.fbcdn.net/
@@||www.facebook.com/*/plugin
@@||www.facebook.com/plugins/
@@||www.facebook.com/rsrc.php
@@||www.facebook.com/ajax/bz
```

Web compatibility observations without these exceptions:
* Embedded FB poss will not appear
* Share and like buttons will not appear

### Twitter embeds

Related issues: https://github.com/brave/browser-laptop/issues/2014, https://github.com/brave/browser-laptop/issues/1208

```
! Block all twitter.com third-party but allow embedded tweets
! Note that options will be added to exclude these filters soon.
||twitter.com$third-party
@@||platform.twitter.com/
@@||syndication.twitter.com;
||twimg.com$third-party,domain=~twitter.com
@@||pbs.twimg.com/
@@||cdn.syndication.twimg.com/
```

Web compatibility observations without these exclusions:
* Embedded tweets show up without images/video. Example page: https://www.buzzfeed.com/jesseszewczyk/martha-stewart-scrambled-eggs-hack
* On some sites, embedded tweets and follow buttons don't show up at all.

### Other

Additional exceptions for cross-site-tracking rules are generally recorded in [brave/adblock-lists/brave-unbreak.txt](https://github.com/brave/adblock-lists/blob/master/brave-unbreak.txt).

## Referrer Policy

Brave currently ships some exceptions to our general referrer policy.  These exceptions are all [open sourced](https://github.com/brave/referrer-whitelist/blob/master/data/ReferrerWhitelist.json).

However, once our [new referrer policy](https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#modified-features-and-functionality) reaches the stable channel, we expect to no longer need any referrer special casing, and plan on removing this list.  This is on track for version 1.12.

## Third Party Cookies

Brave makes a small number of exceptions, where we enable third party cookies to unbreak sites.  These exceptions are all narrow, meaning we enabling sending cookies to a specific third party, when on a specific first party.  These exceptions can be found in Brave's [source code](https://github.com/brave/brave-core/blob/master/chromium_src/components/content_settings/core/common/cookie_settings_base.cc#L39).  We are working on a new issue that we expect will reduce or completely remove the need for these exceptions.  The details and status of that work can be found in [issue #8514](https://github.com/brave/brave-browser/issues/8514).