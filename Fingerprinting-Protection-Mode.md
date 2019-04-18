*This article is also available here but less up to date: https://community.brave.com/t/all-about-fingerprinting-protection-mode*

## What is Fingerprinting Protection?

Fingerprinting Protection is a privacy feature that makes it harder for sites to track you while you browse.

**Warning:** enabling Fingerprinting Protection might cause some sites to display incorrectly.

## Technical Details

Brave includes best-effort defense against [browser fingerprinting](https://www.torproject.org/projects/torbrowser/design/#fingerprinting-linkability). Broadly speaking, browser fingerprinting is the detection of browser and operating system features that differ between users for the purpose of covertly identifying users and tracking them across the web. Although fingerprinting attacks will always be possible, it is worthwhile for us to make these attacks as slow / costly / difficult as possible.

Because most browser fingerprinting defense requires disabling web features that are required for many sites to work properly, it is implemented as off-by-default for now (can be turned on in `about:preferences` globally, or on a per-site basis in the Bravery panel). We will consider turning it on-by-default when we have fingerprinting detection heuristics with a sufficiently-low false positive rate.

**UPDATE (8/18/17)**: As of Brave 0.20.x, Fingerprinting Protection Mode will be enabled by default for all **third-party** content.

## Fingerprinting methods blocked in Fingerprinting Protection Mode

* [Canvas fingerprinting](https://www.browserleaks.com/canvas): it should report a fixed value on tests like panopticlick
* [WebGL fingerprinting](https://amiunique.org/faq): it should report as undefined on tests like panopticlick
* [AudioContext fingerprinting](https://audiofingerprint.openwpm.com/)
* [WebRTC IP leakage](https://github.com/brave/browser-laptop/issues/260)
* [SVG fingerprinting](https://github.com/brave/browser-laptop/issues/10288) (specifically, the `SVGTextContentElement.prototype.getComputedTextLength` and `SVGPathElement.prototype.getTotalLength` methods)
* [HSTS fingerprinting](https://github.com/brave/brave-browser/issues/3419)

## Privacy protection enabled regardless of whether Fingerprinting Protection Mode is on

This list is not complete. See https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove) for other things which are disabled in Brave but not in Chrome.

* 3rd party cookies and referers blocked by default due to the third party tracking risk
* User-Agent is set to Chrome except on a few sites that need it for major functionality to work to prevent sites from using Brave's UA as a tracking mechanism.
* `navigator.plugins` and `navigator.mimeTypes` is empty unless you've enabled Flash to trigger HTML5 fallback for Flash whenever possible.
* Connections to known tracking domains are blocked via the Tracking Protection library
* [Battery Status API](https://github.com/brave/browser-laptop/issues/1885) is disabled because the battery level can be used as a tracking signal.
* `navigator.credentials` is disabled on desktop prior to C73; we are re-enabling it to support [webauthn](https://hacks.mozilla.org/2018/01/using-hardware-token-based-2fa-with-the-webauthn-api/).
* Web USB and Web Bluetooth are disabled on desktop due to us not seeing much benefit to enabling it right now
* We are also planning on disabling client-hints, see https://github.com/brave/brave-browser/issues/3539#issuecomment-483826927 for rationale

## How to check that it's working

See https://community.brave.com/t/how-do-i-turn-on-browser-fingerprinting-protection-and-make-sure-that-its-working.

## Why does panopticlick.eff.org or some other site say that I am fingerprintable?

Although useful for raising awareness of fingerprinting techniques, sites like Panopticlick are not a perfect indicator of how fingerprintable your browser is. Some known limitations are:

* Panopticlick only reports your uniqueness relative to the population of users visiting Panopticlick, which is almost certainly skewed relative to the entire population of users on the web. For instance, imagine that a very large number of Tor Browser users visit Panopticlick because they're trying to test their Tor Browser privacy settings. If you then visit Panopticlick in Chrome with default settings, you will then appear as more identifiable than Tor Browser users despite the fact that Chrome with default settings is more popular than Tor Browser overall. Similarly, because many Panopticlick users care about privacy and turn on Do Not Track, Panopticlick reports that users are *less* unique when they have DNT turned on than off, even though probably [less than 12%](https://blog.mozilla.org/netpolicy/2013/05/03/mozillas-new-do-not-track-dashboard-firefox-users-continue-to-seek-out-and-enable-dnt/) of web users have DNT enabled.
* **[EDIT (12/11/18): This appears to no longer be true.]** Panopticlick also compares you against old browsers. For instance, if the plurality of Panopticlick visits were from people using Firefox 3 many years ago, then a person using Firefox 3 could appear as not-very-identifiable even though there are extremely few Firefox 3 users on the web in 2017 (or at least one would hope).
* Panopticlick does not account for the fact that randomized fingerprint values are an effective way to prevent real-world fingerprinting. For instance, if Brave browser randomized canvas fingerprints on every page request, then it would be impossible for a site to track a specific Brave user across requests using canvas fingerprinting. However, because the randomized values would be unique, Panopticlick would report Brave as being highly canvas-fingerprintable.

**[EDIT (12/11/18): This may no longer work.]** One way to "trick" Panopticlick is to open the site in various Brave session tabs and re-run the fingerprinting test. Panopticlick will then report that your Brave configuration is less identifiable because there have been other "users" visiting the site with the same configuration.

## TODO

* Decrease JS timer resolution
* Limit fonts fingerprinting: https://github.com/brave/brave-browser/issues/816
* Limit fingerprinting via viewport/screen size: https://github.com/brave/brave-browser/issues/720
* Lots more at https://github.com/brave/brave-browser/labels/feature%2Fshields%2Ffingerprint and https://github.com/brave/brave-browser/labels/privacy%2Ftracking

## Further reading
* [W3C note in progress on browser fingerprinting](https://w3c.github.io/fingerprinting-guidance/)
* [Chromium security page on client-identification mechanisms](https://sites.google.com/a/chromium.org/dev/Home/chromium-security/client-identification-mechanisms)
* [Recent study on fingerprinting methods observed in the wild](http://randomwalker.info/publications/OpenWPM_1_million_site_tracking_measurement.pdf)
* [Study on Web API methods used in the wild, and their relation to tracking](https://www.cs.uic.edu/%7Epsnyder/static/papers/Browser_Feature_Usage_on_the_Modern_Web.pdf)