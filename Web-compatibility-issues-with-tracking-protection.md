This is a list of domains we have had to intentionally exclude from tracking protection due to site features breaking and user complaints. Note that these sites are not allowed access to third party cookies/storage.

### Facebook domains

Related issues: https://github.com/brave/browser-laptop/issues/780, https://github.com/brave/browser-laptop/issues/880, https://bugzilla.mozilla.org/show_bug.cgi?id=1226498, https://github.com/brave/browser-laptop/issues/2014

Notes: We may be able to fix some of these issues by loading FB's SDK from a local file or polyfill instead of calling out to their domains. The SDK is often used for "Login with Facebook" buttons.

Domains which are excluded (desktop/android only?):
* connect.facebook.net
* connect.facebook.com
* staticxx.facebook.com
* www.facebook.com
* scontent.xx.fbcdn.net
* scontent-sjc2-1.xx.fbcdn.net (for embedded fb images)

Web compatibility observations without these exclusions:
* quora.com fb login is broken
* same with digg.com
* same with fitbit.com
* share and like buttons appear to still work
* fb login on airbnb.com works fine

### Twitter domains

Related issues: https://github.com/brave/browser-laptop/issues/2014, https://github.com/brave/browser-laptop/issues/1208

Domains which are excluded (desktop/android only?):
* platform.twitter.com
* syndication.twitter.com
* pbs.twimg.com
* cdn.syndication.twimg.com

Web compatibility observations without these exclusions:
* Embedded tweets show up without images/video. Example page: https://www.buzzfeed.com/jesseszewczyk/martha-stewart-scrambled-eggs-hack
* On some sites, embedded tweets don't show up at all.

### Other

These were added in https://github.com/brave/tracking-protection/blame/132ac934be9dd0113a4bb4d041f7602a72c60662/data/disconnect.json#L9348-L9375 and apply to all platforms. Serg says the websites were broken without these exceptions. Note that the exceptions are double-keyed (applies only for a specific subresource domain on a specific first-party domain).

TODO: Check if sites are still broken in any way without these exceptions.

Domains which are excluded:
* 2mdn.net on https://www.cnet.com/
* pcache.alexa.com on http://www.alexa.com/ (shouldn't be needed any more if the domains are considered first party based on eTLD+1)
* google-analytics.com on https://www.saturn.at/ and https://www.mediamarkt.at/




