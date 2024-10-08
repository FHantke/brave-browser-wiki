_**NOTE: this page is a work in progress! It should by no means be considered a "final" or exhaustive list of things we have removed.**_

--------
- [How it works](#how-it-works)
- [What Chromium features are removed for privacy/security reasons?](#what-chromium-features-are-removed-for-privacysecurity-reasons)
    - [Services & Features We Disable Entirely](https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#services--features-we-disable-entirely)
    - [Services We Proxy Through Brave Servers](https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#services-we-proxy-through-brave-servers)
    - [Modified Features and Functionality](https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#modified-features-and-functionality)
- [How does Brave compare to ungoogled-chromium?](#how-does-brave-compare-to-ungoogled-chromium)

--------

Brave for Desktop and Android is built on top of the [open-source Chromium project](https://www.chromium.org/chromium-projects). We add features on top of what is already there and we also remove features or pieces of the code. These deviations we make that touch the core Chromium code are done via patching.

The scope of this page is to cover Chromium functionality that is modified in some way. Brave Browser has its own features which are not covered by this document.

Chromium is not the same as Google Chrome. For some differences, see https://chromium.googlesource.com/chromium/src/+/master/docs/chromium_browser_vs_google_chrome.md. 

## How it works
If you wanted to do an audit of the code, you would start with the [`brave-browser`](https://github.com/brave/brave-browser) repository. [Our wiki has instructions](https://github.com/brave/brave-browser/wiki) about what steps need to be done to perform a build after cloning the source.

### Chromium source is fetched
The gclient utility (part of depot tools) will fetch the official Chromium source code. The tag that is fetched is [captured in our package.json](https://github.com/brave/brave-core/blob/master/package.json). For example, `.config.projects.chrome.tag` at the time of this writing is `116.0.5845.51`. All of the source code from Chromium will be downloaded into the `./src/` folder.

### Brave code is fetched
As part of the setup process, we also fetch our own code. The [`brave-core` repository](https://github.com/brave/brave-core) has the code that makes the browser Brave. The branch that should be checked out is also contained in that [package.json](https://github.com/brave/brave-browser/blob/master/package.json). There is also a [`DEPS` file in `brave-core`](https://github.com/brave/brave-core/blob/master/DEPS) that pulls in sub-dependencies. For example, the [`omaha`](https://github.com/brave/omaha) and [`sparkle`](https://github.com/brave/Sparkle) dependencies are both used for product updates.

### Hooks are run
After the gclient sync runs and fetches all the code (including `brave-core`), the hooks are run. One of the hooks that runs applies the patches ([which you can see here](https://github.com/brave/brave-core/tree/master/patches)) that are contained in `brave-core`. If you'd like to know more details about HOW the patching works, you can [take a peek at our patching wiki page](https://github.com/brave/brave-browser/wiki/Patching-Chromium).

## What Chromium features are removed for privacy/security reasons?

### Services & Features We Disable Entirely

- [Google accounts integration ("<abbr title="Google Accounts and ID Administration">GAIA</abbr>") is disabled](https://github.com/brave/brave-core/pull/512)
- [All features that send data to Google are removed from settings](https://github.com/brave/brave-core/pull/244)
- [DNS prefetching is disabled](https://github.com/brave/brave-core/pull/340)
- [Chrome Google URL Tracker is disabled](https://github.com/brave/brave-core/pull/248)
- [Domain service reliability is disabled](https://github.com/brave/brave-core/pull/246)
- [Inline extensions are disabled](https://github.com/brave/brave-browser/issues/614)
- [Background sync is disabled](https://github.com/brave/brave-browser/issues/515)
- [Hyperlink `ping` attribute is disabled](https://github.com/brave/brave-browser/issues/764)
- [Disable WebBluetooth API](https://github.com/brave/brave-core/pull/114)
- [WebRTC debug log uploading is disabled](https://github.com/brave/brave-core/pull/809)
- [Uploading settings after resetting profile is disabled](https://github.com/brave/brave-core/pull/745)
- [Retrieving OEM default settings after resetting profile is disabled](https://github.com/brave/brave-core/pull/978)
- [Tracing crash log uploading is disabled](https://github.com/brave/brave-browser/issues/2121)
- [Google Cloud Messaging is disabled](https://github.com/brave/brave-browser/issues/1716)
- [Firebase Cloud Messaging is disabled](https://github.com/brave/brave-core/pull/908)
- [Push client channel updates are disabled](https://github.com/brave/brave-core/pull/912)
- [Network time tracker is disabled](https://github.com/brave/brave-core/pull/792)
- [Google-assisted address normalization is disabled](https://github.com/brave/brave-core/pull/769)
- [Specific features are disabled on startup via the CLI](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc) (search for `disabled_features`)
- [Remove dl.google.com repository from Linux packages](https://github.com/brave/brave-core/pull/1078)
- [Disable metrics reporting](https://github.com/brave/brave-core/pull/2029)
- [Disable Reporting API](https://github.com/brave/brave-browser/issues/7956). Note that the JS API for Reporting API ([ReportingObserver](https://developer.mozilla.org/en-US/docs/Web/API/ReportingObserver)) is enabled but is a no-op in Brave, see below.
- [Disable Scroll To Text Fragment](https://github.com/brave/brave-core/pull/4548/commits/3221538c3b2939d11a3074be3d5c8f44b2540a6c)
- [Disable Motion Sensors](https://github.com/brave/brave-browser/issues/4789)
- [Disable navigator.credentials](https://github.com/brave/brave-browser/issues/13#issuecomment-376991976)
- [Disable Android OTP integration](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc)
- [Disable SXG](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc)
- [Disable NFC](https://github.com/brave/brave-core/blob/master/renderer/brave_content_renderer_client.cc#L30)
- [Disable WebBundles](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc)
- [Disable Client Hints (lang)](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc#L221)
- [Disable Direct / Raw Sockets](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc#L219)
- [Disable Idle Detection](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc)
- [Disable Notification Triggers](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc)
- [Disable File System API](https://github.com/brave/brave-browser/issues/11407)
- [Disable Digital Goods API](https://github.com/brave/brave-core/blob/master/chromium_src/third_party/blink/renderer/core/origin_trials/origin_trials.cc#L23)
- [Disable Federated Learning of Cohorts (FLoC)](https://github.com/brave/brave-browser/issues/14942)
- [Disable Network Information API](https://github.com/brave/brave-browser/issues/20122)
- [SCT auditing](https://github.com/brave/brave-core/blob/f21d39fd1bf651c586ac157e9213b217331c3033/chromium_src/chrome/common/chrome_features.cc#L20)
- [Site affiliation fetcher](https://github.com/brave/brave-core/pull/18153) (part of the password manager)
- [Disables kOptimizationGuideFetchingForSRP](https://github.com/brave/brave-core/pull/19457)
- [Disable Web Environment Integrity](https://github.com/brave/brave-core/pull/19476)
- [Disable Popular Sites](https://github.com/brave/brave-core/pull/24511)

### Services We Proxy Through Brave Servers

_Google does not receive any information about which client is performing these requests (not even your IP address)._

- [SafeBrowsing requests are proxied](https://github.com/brave/brave-core/pull/108)
- [Geolocation requests are proxied](https://github.com/brave/brave-core/pull/19)
- [Plugin updates are proxied](https://github.com/brave/brave-core/pull/680)
- [Certificate revocation requests are proxied](https://github.com/brave/brave-core/pull/997)
- [Requests for CRLSets are proxied](https://github.com/brave/brave-core/pull/1581)
- [Requests for component updates are proxied](https://github.com/brave/brave-core/pull/1581)
- [Requests for spellcheck dictionaries are proxied](https://github.com/brave/brave-core/pull/11917)
- [Requests in devtools are proxied](https://github.com/brave/brave-core/pull/5500)

#### Proxied endpoints
- `https://dl.google.com/release2/chrome_component/*crl-set*`
- `https://*.gvt1.com/edgedl/release2/chrome_component/*`
- `https://*.gvt1.com/edgedl/chrome/dict/*.bdic`
- `https://storage.googleapis.com/update-delta/hfnkpimlhhgieaddgfemjhofmfblmnib/.+crxd`
- `https://safebrowsing.googleapis.com/`
- `https://sb-ssl.google.com/`
- `https://safebrowsing.google.com`
- `https://ssl.gstatic.com`
- `https://gstatic.com`
- `https://update.googleapis.com`
- `https://chrome-devtools-frontend.appspot.com`
- `https://clients2.googleusercontent.com`
- `https://clients2.google.com`
- `https://clients4.google.com`
- `https://chrome-devtools-frontend.appspot.com`
- `https://accounts.google.com`
- `https://*.infura.io`
- `https://*.gvt1.com/edgel/chromewebstore/*/*`
- `https://*.gvt1.com/edgedl/release2/*/*`
- `http://dl.google.com/release2/*/*`

### Modified Features and Functionality
- Cookies:
  - Have a [maximum lifetime](https://github.com/brave/brave-browser/issues/3443) of 7 days when set through Javascript and 6 months for when set through a HTTP header.
  - Session Cookies are [cleaned up on restart](https://github.com/brave/brave-browser/issues/28379) if "Continue where you left off" mode is enabled (which is default in Brave).
  - Third-party cookies are always blocked at the HTTP header level, but we give JavaScript access to partitioned ephemeral storage to pages ([learn more](https://brave.com/privacy-updates/7-ephemeral-storage/) about partitioned ephemeral storage).
- The Battery API always [returns a fixed value](https://github.com/brave/brave-core/pull/567).
- Referrer values are capped to `strict-origin-when-cross-origin` and can only be tightened by referrer policy, not weakened. In addition, cross-origin requests from a `.onion` service have an empty `Referer` header and a `null` `Origin` header just like the Tor Browser.
- `.onion` entries are replaced with `"null"` in `document.location.ancestorOrigins()` unless such entries are same-origin with the innermost frame.
- [Hangouts extension](https://source.chromium.org/chromium/chromium/src/+/main:chrome/browser/resources/hangout_services/manifest_v3.json) is enabled by default on Desktop to make sure Google Hangouts works as expected on Brave. We disable log uploading to Google's servers: https://github.com/brave/brave-browser/issues/1993. You can turn it off by toggling the switch in brave://settings/extensions. Update: this extension is now disabled by default on Nightly: https://github.com/brave/brave-core/pull/24583, and will eventually be disabled/removed everywhere.
- Media Router (Chromecast) is enabled by default for new profiles on Desktop. You can turn it off by toggling the switch in brave://settings/extensions. We modify the SSDP user agent to not leak the fact that it's a Brave user: https://github.com/brave/brave-core/pull/22140
- Download protection remote lookups omit URLs and filenames (https://github.com/brave/brave-core/pull/6763).
- Have StorageManager.estimate report a fixed value [#11543](https://github.com/brave/brave-browser/issues/11543)
- Many features have randomness added or values generalized as a defense against fingerprinting, including:
    * [Canvas readback methods](https://github.com/brave/brave-browser/issues/9186)
    * [User Agent](https://github.com/brave/brave-browser/issues/9190#issuecomment-707172886), follow ups in [#12097](https://github.com/brave/brave-browser/issues/12097), [#12638](https://github.com/brave/brave-browser/issues/12638), [#14740](https://github.com/brave/brave-browser/issues/14740)
    * [enumerateDevices](https://github.com/brave/brave-browser/issues/11271)
    * [Web Audio Serialization](https://github.com/brave/brave-browser/issues/9187)
    * [WebGL Debug](https://github.com/brave/brave-browser/issues/9188)
    * [Plugins](https://github.com/brave/brave-browser/issues/9435)
    * [hardwareConcurrency](https://github.com/brave/brave-browser/issues/10808)
    * [deviceMemory](https://github.com/brave/brave-browser/issues/12348)
- The list of hostnames with pinned CA certificates is replaced with a [Brave-specific one](https://github.com/brave/brave-core/blob/master/chromium_src/net/tools/transport_security_state_generator/input_file_parsers.cc).
- [Restore gesture requirement for async clipboard write access](https://github.com/brave/brave-browser/issues/16890)
- [Dangerous download warnings are always shown when Safe Browsing is OFF](https://github.com/brave/brave-browser/issues/28079), but [a flag to disable the warnings](https://github.com/brave/brave-core/pull/18877) is available for advanced users.
- [Functionality from the Chromium side panel has been merged into the Brave Sidebar](https://github.com/brave/brave-core/pull/13769).
- [Web Serial API is OFF](https://github.com/brave/brave-browser/issues/13902) but [a flag to enable](https://github.com/brave/brave-browser/issues/24404) is available for advanced users.
- Enhanced the [geolocation permission dialog](https://github.com/brave/brave-core/pull/23278) to inform the user whether the site they are visiting has requested location data with the `enableHighAccuracy` option.
- Reporting Observers are enabled but don't work (calling will no-op). See [here](https://github.com/brave/brave-core/pull/4578) for where it was disabled and see [here](https://github.com/brave/brave-core/pull/24726) for where it was re-enabled but made to do nothing.

### Comments

Some of the above (along with other issues) were previously tracked in https://github.com/brave/brave-browser/issues/13.

You may notice some requests to Google domains. Some of these, such as `clients*.google.com` and `update.googleapis.com` are needed to check for extension updates if you installed extensions.

## How does Brave compare to `ungoogled-chromium`?
Description of [`ungoogled-chromium`](https://github.com/Eloston/ungoogled-chromium), per their GitHub page:
> *ungoogled-chromium is Google Chromium*, sans integration with Google. It also features some tweaks to enhance privacy, control, and transparency _(almost all of which require manual activation or enabling)_.

We have [an issue captured](https://github.com/brave/brave-browser/issues/1431) for pulling in relevant patches from the `ungoogled-chromium` project. The `ungoogled-chromium` project similarly has an [issue captured](https://github.com/Eloston/ungoogled-chromium/issues/543) where they mention pulling in patches from Brave.
