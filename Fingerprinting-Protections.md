## What is Fingerprinting Protection?

Fingerprinting Protection is a privacy feature that makes it harder for sites to track you while you browse.

## Technical details

Brave includes best-effort defense against [browser fingerprinting](https://www.torproject.org/projects/torbrowser/design/#fingerprinting-linkability). Broadly speaking, browser fingerprinting is the detection of browser and operating system features that differ between users for the purpose of covertly identifying users and tracking them across the web. Although fingerprinting attacks will always be possible, it is worthwhile for us to make these attacks as slow / costly / difficult as possible.

Brave includes two types of fingerprinting protections, (i) blocking, removing or modifying APIs, to make Brave instances look as similar as possible, and (ii) randomizing values from APIs, to prevent cross session and site linking (e.g. making Brave instances look different to websites each time).

In cases where we block, remove or modify API behavior, we attempt to return empty, or non-identifying values, that have the "shape" of expected values, to minimize web compatibility issues.

In cases where we randomize API values, we attempt to make modifications that are imperceivable to humans, but distinguishing to computers / fingerprinters.  These randomization values are derived from a seed that changes per session, and per eTLD+1.  Third party frames and script share the seed value of the top level, eTLD+1 domain. This approach is especially useful in fingerprinters that hash together a large number of semi-identifiers into a single identifier, since randomizing just one value "poisons" the entire fingerprint.


## Why does panopticlick.eff.org or some other site say that I am fingerprintable?

These sites wrongly detect Brave as identifiable because they are designed to measure a different form of fingerprinting protection than Brave uses. Most tools try to make as many browsers look identical as possible, and sites like panopticlick.eff.org look to see if your browser matches any they've seen previously.  If not, then they determine that you're fingerprintable.

Brave's system for protecting users against fingerprinting works differently. Instead of trying to make Brave users look identical (a goal that is not achievable for many users in many cases, without breaking websites or turning off useful browser functionality), Brave tries to make you look as _different_ as possible, for each website, for each session.  This prevents browsers from identifying you when you visit other sites, or when you return to the same site in the future.

Brave uses this anonymity-through-randomization approach for several reasons including i) it better protects users with browser / computer / language / etc configurations, and ii) its more web compatible, since it doesn't require disabling browser features.

More information about Brave's "privacy through randomization" systems can be found in the following blog posts:

- [What’s Brave Done For My Privacy Lately? Episode #3: Fingerprint Randomization](https://brave.com/whats-brave-done-for-my-privacy-lately-episode3/)
- [What’s Brave Done For My Privacy Lately? Episode #4: Fingerprinting Defenses 2.0
](https://brave.com/whats-brave-done-for-my-privacy-lately-episode-4-fingerprinting-defenses-2-0/)


## How to check that it's working

See https://community.brave.com/t/fingerprinting-how-do-we-know-its-actually-working/134536

## Differences between "standard" and "strict" protections

Brave has two levels of fingerprinting protections.  In the default, "standard" configuration, Brave adds subtle noise to APIs commonly used for fingeprinting. This small amount of noise is enough to make you look different to fingerprinting scripts, without breaking websites, and will provide good protections against web-scale online trackers.

Brave also includes a "strict" option. When set to "strict" mode, Brave only returns random values from APIs commonly used by fingerprinters.  This provides a higher level of protection against highly determined attackers, who may attempt statistical and / or targeted attacks to identify users. This mode will also break websites who depend on these features to work correctly.

## Fingerprinting methods randomized

* Canvas: [#9186](https://github.com/brave/brave-browser/issues/9186)
* WebGL: [#9188](https://github.com/brave/brave-browser/issues/9188)
* Web Audio: [#9187](https://github.com/brave/brave-browser/issues/9187)
* Plugins: [#9435](https://github.com/brave/brave-browser/issues/9435), [#10597](https://github.com/brave/brave-browser/issues/10597)
* Hardware Concurrency: [#10808](https://github.com/brave/brave-browser/issues/10808)
* Enumerate Devices (order): [#11271](https://github.com/brave/brave-browser/issues/11271)

Progress on the randomization for additional fingerprint-able sources can be found here: https://github.com/brave/brave-browser/issues/8787

## Fingerprinting methods blocked

* [WebRTC IP leakage](https://github.com/brave/browser-laptop/issues/260)
* [HSTS fingerprinting](https://github.com/brave/brave-browser/issues/3419)
* [Client Hints](https://github.com/brave/brave-browser/issues/3539)
* [Battery Status API](https://github.com/brave/browser-laptop/issues/1885) is disabled because the battery level can be used as a tracking signal.
* Web Bluetooth is disabled on desktop due to us not seeing much benefit to enabling it right now

## TODO
* Limit fonts fingerprinting: https://github.com/brave/brave-browser/issues/816
* Limit fingerprinting via viewport/screen size: https://github.com/brave/brave-browser/issues/720

## Further reading
* [W3C note in progress on browser fingerprinting](https://w3c.github.io/fingerprinting-guidance/)
* [Chromium security page on client-identification mechanisms](https://sites.google.com/a/chromium.org/dev/Home/chromium-security/client-identification-mechanisms)
* [Recent study on fingerprinting methods observed in the wild](http://randomwalker.info/publications/OpenWPM_1_million_site_tracking_measurement.pdf)
* [Study on Web API methods used in the wild and their relation to tracking](https://www.cs.uic.edu/%7Epsnyder/static/papers/Browser_Feature_Usage_on_the_Modern_Web.pdf)