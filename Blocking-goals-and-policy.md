# Privacy Protections In Brave

Brave modifies how websites execute, and what network requests websites can make.  Brave does this to protect user privacy, improve website performance, and generally improve the experience for users.  This document describes both the policy Brave uses to decide what to block and modify, and the techniques Brave uses to approximate the policy in the browser.  We can only at best "approximate" the policy because of the wide variety in how websites work, and the efforts trackers go to circumvent user protections.

## Blocking Goals and Policy

Brave's default settings attempt to protect user privacy by blocking third-party advertising.  Brave's goal in doing so is not to block the advertisement images themselves, but to block the tracking such advertisements cause. In practice, it is difficult-to-impossible to distinguish third-party advertising with third-party tracking, so Brave blocks both.

In Standard blocking mode (the default), Brave does not apply network-level filter list blocking to first-party requests. In Aggressive blocking mode, both third-party and first-party requests are blocked. Read more in the [blog post](https://brave.com/privacy-updates/9-web-compat-blocking/).

Brave will always attempt to block anti-adblock behavior. 

### Regional Lists

Default-enabled regional adblock lists operate in Standard blocking mode. This means that Brave will not apply network-level blocking to first-party requests and ads. Historically, a lot of default-enabled regional lists used to operate in Aggressive blocking mode; we are working on transitioning them over to Standard mode. We plan to offer a [toggle in settings](https://github.com/brave/brave-browser/issues/40870) that users can enable to get back the legacy behavior.

Optional and custom lists always operate in Aggressive blocking mode. 

### First-party blocking

While Brave does not intentionally target first-party advertising for blocking, Brave doesn't consider it an error either.  In other words, Brave does not try to block first party ads, but won't take efforts to unblock first-party ads if they're being blocked by other steps.

Similarly, Brave also blocks code that attempts to identify (fingerprint) users based on unique browser characteristics, hardware configuration and similar semi-unique data points.  Such identification techniques are just as harmful to users as traditional cookie-based tracking.

Finally, Brave intentionally blocks website behaviors that are harmful to users, whether or not those behaviors are privacy-harming.  For example, Brave blocks crypto-mining scripts.  These scripts use the user's computer in an intensive manner to try and earn money for the hosting website, and result in degraded performance and reduced battery life.  Crypto mining scripts are only one such example, but when possible, Brave will modify websites and requests to improve the user experience.

Brave allows first-party analytics scripts as long as the script does not engage in clear cases of user-harmful behaviour as defined above.

Brave also aims to empower users who wish to block first-party advertising and tracking scripts, even though doing so often comes at an increased risk of website breakage. Many Shields settings have two levels of strength to accommodate these use cases for advanced users, and Brave's adblocker is extensible with custom filter list entries and subscriptions.

## Blocking Techniques and Methods

Brave makes a best effort attempt to enforce the above policy, through a number of steps.  The majority of the below described techniques are controlled by the "Shields" panel in Brave, and can be disabled if and when the user desires too.  Because of platform restrictions, Brave is not able to use all of these techniques on iOS, though we are constantly looking for ways of increasing protections on that platform.

First, Brave blocks the most common tracking mechanism, sending cookies to third party resources.  By default, Brave never sends cookies to third parties, nor respects storage setting and reading operations by scripts operating in the third party contexts.

Second, Brave modifies the referrer header when making cross origin requests.  Brave "lies" on these requests, and says the request was being issued from the same domain being requested, instead of the true, cross-domain origin.

Third, Brave prevents third party frames from tracking users through [passive finger printing techniques](https://github.com/brave/brave-browser/wiki/Fingerprinting-Protections).  Brave modifies or returns false values for a number of Web API endpoints that can be used to identify users (e.g. Canvas API, WebGL, Web Audio API, etc.).  Brave by default only does this in third party contexts, but can be modified to perform the same protections globally, or not-at-all.

Fourth, Brave pulls from a variety of community developed filter lists, or lists of URLs used for carrying out advertising or tracking.  These lists include [EasyList and EasyPrivacy](https://github.com/easylist/easylist), lists generated by the [uBlock Origin](https://github.com/uBlockOrigin) project, and lists maintained by Brave itself.  URLs identified by these lists are either blocked, or have their resources modified, to protect users. Brave also uses lists to block coin miners and scripts that engage in "notifications spam."  The [current, full set of filter lists Brave uses](https://github.com/brave/adblock-resources/blob/master/filter_lists/list_catalog.json) can be found in our source.

## "Aggressive" blocking mode features

In this mode, Brave will block any identified first-party content specified by default filter lists. [CNAME uncloaking](https://brave.com/privacy-updates/6-cname-trickery/) is also enabled to block additional resources that may normally go undetected.

## Lists used

An up-to-date catalog of Brave's default and optional adblock lists can be found [in the list catalog](https://github.com/brave/adblock-resources/blob/master/filter_lists/list_catalog.json), in the adblock-resources repository. In the browser, this can be found on the `brave://settings/shields/filters` settings page.

* **Regional adblock lists**

Brave has toggleable country-specific adblock filter options. These filter lists are imported from various list authors and will offer added protection not covered by Easylist (which is an English-specific list).

**Mobile:** Can be configured in the `settings/privacy`<br> 
**Desktop:** Filters can be changed using `brave://settings/shields/filters`<br>

* **EasyList**

Primary filter list that removes most advertisements from webpages.<br>
**Type of Rules**: `#network` `#cosmetic`<br>
**Address:** https://easylist.to/easylist/easylist.txt<br>
**Support:** https://forums.lanik.us/

* **EasyPrivacy**

Removes tracking scripts, information collectors and other tracking elements. Protecting your privacy.<br>
**Type of Rules**: `#network`<br>
**Address:** https://easylist.to/easylist/easyprivacy.txt<br>
**Support:** https://forums.lanik.us/

* **Brave-unbreak (Brave specific list)**

Brave-generated filter rules to address web compatibility issues unique to Brave, and to target anti-adblock resources.<br>
**Type of Rules**: `#network`<br>
**Address:** https://github.com/brave/adblock-lists/blob/master/brave-unbreak.txt<br>
**Support:** https://github.com/brave/adblock-lists/issues

* **uBlock Lists**

uBlock Origin community filters to counter broken sites and to address privacy scripts.<br>
**Type of Rules**: `#network` `#cosmetic`<br>
**Address:** https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/filters.txt<br>
**Address:** https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/filters-2020.txt<br>
**Address:** https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/badware.txt<br>
**Address:** https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/privacy.txt<br>
**Address:** https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/resource-abuse.txt<br>
**Address:** https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/unbreak.txt<br>
**Support:** https://github.com/uBlockOrigin/uAssets/issues

* **URLhaus Malicious URL Blocklist**

Blocks websites that exist solely to serve malware.<br>
**Type of Rules**: `#network`<br>
**Address:** https://raw.githubusercontent.com/curbengh/urlhaus-filter/master/urlhaus-filter-online.txt<br>
**Support:** https://gitlab.com/curben/urlhaus-filter

* **Peter Lowe's ad and tracking server list**

Blocks ad servers, tracking servers, malware servers, and anti-adblock servers.<br>
**Type of Rules**: `#network`<br>
**Address:** https://pgl.yoyo.org/adservers/serverlist.php?hostformat=adblockplus&showintro=1&mimetype=plaintext<br>
**Support:** https://pgl.yoyo.org/adservers/

* **Brave Social**

Prevents third-party social media elements from loading to protect user privacy.<br>
**Type of Rules**: `#network`<br>
**Address:** https://raw.githubusercontent.com/brave/adblock-lists/master/brave-lists/brave-social.txt<br>
**Support:** https://github.com/brave/adblock-lists

Please see https://github.com/brave/adblock-resources/blob/master/filter_lists/list_catalog.json for a full catalog.

## iOS implementation (and deviations from Android / Desktop)
Per Apple's [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/#software-requirements) (section `2.5.6`):
> _Apps that browse the web must use the appropriate WebKit framework and WebKit Javascript._

The iOS version of Brave is constrained by what is available to [WKWebview](https://developer.apple.com/documentation/webkit/wkwebview). Brave utilizes the [Content Blocker extension](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/ContentBlocker.html) to implement a subset of the lists available to Android and Desktop, called [Slim List](https://github.com/brave/slim-list-lambda). Each week, the code from [Slim List Lambda](https://github.com/brave/slim-list-lambda) will execute from a Brave host and generate a new list for use in iOS. This list is then uploaded to AWS S3 where the iOS browser will fetch it at run-time ([at program launch and then every 6 hours afterwards](https://github.com/brave/brave-ios/pull/3130)). We're looking into deprecating this.