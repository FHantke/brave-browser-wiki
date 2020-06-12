_**NOTE: this page is a work in progress! It should by no means be considered a "final" or exhaustive list of things we have removed.**_

--------
- [How it works](#how-it-works)
- [What Chromium features are removed for privacy/security reasons?](#what-chromium-features-are-removed-for-privacysecurity-reasons)
    - [Services & Features We Disable Entirely](https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#services--features-we-disable-entirely)
    - [Services We Proxy Through Brave Servers](https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#services-we-proxy-through-brave-servers)
    - [Modified Features and Functionality](https://github.com/brave/brave-browser/wiki/Deviations-from-Chromium-(features-we-disable-or-remove)#modified-features-and-functionality)
- [How does Brave compare to ungoogled-chromium?](#how-does-brave-compare-to-ungoogled-chromium)

--------

Brave for desktop is built on top of the [open-source Chromium project](https://www.chromium.org/chromium-projects). We add features on top of what is already there and we also remove features or pieces of the code. These deviations we make that touch the core Chromium code are done via patching.

Chromium is not the same as Google Chrome. For some differences, see https://chromium.googlesource.com/chromium/src/+/master/docs/chromium_browser_vs_google_chrome.md. 

## How it works
If you wanted to do an audit of the code, you would start with the [`brave-browser`](https://github.com/brave/brave-browser) repository. [Our wiki has instructions](https://github.com/brave/brave-browser/wiki) about what steps need to be done to perform a build after cloning the source

### Chromium source is fetched
The gclient utility (part of depot tools) will fetch the official Chromium source code. The tag that is fetched is [captured in our package.json](https://github.com/brave/brave-browser/blob/master/package.json) (for example, `70.0.3538.35`). All of the source code will be downloaded into the `./src/` folder

### Brave code is fetched
As part of the setup process, we also fetch our own code. The [`brave-core` repository](https://github.com/brave/brave-core) has the code that makes the browser Brave. The branch that should be checked out is also contained in that [package.json](https://github.com/brave/brave-browser/blob/master/package.json). There is also a [`DEPS` file in `brave-core`](https://github.com/brave/brave-core/blob/master/DEPS) that pulls in sub-dependencies ([such as the `brave-extension`](https://github.com/brave/brave-extension))

### Hooks are run
After the gclient sync runs and fetches all the code (including `brave-core`), the hooks are run. One of the hooks that runs applies the patches ([which you can see here](https://github.com/brave/brave-core/tree/master/patches)) that are contained in `brave-core`. If you'd like to know more details about HOW the patching works, you can [take a peek at our patching wiki page](https://github.com/brave/brave-browser/wiki/Patching-Chromium)

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
- [Battery API is disabled](https://github.com/brave/brave-core/pull/114)
- [WebBluetooth API is disabled](https://github.com/brave/brave-core/pull/114)
- [WebRTC debug log uploading is disabled](https://github.com/brave/brave-core/pull/809)
- [Uploading settings after resetting profile is disabled](https://github.com/brave/brave-core/pull/745)
- [Retrieving OEM default settings after resetting profile is disabled](https://github.com/brave/brave-core/pull/978)
- [Tracing crash log uploading is disabled](https://github.com/brave/brave-browser/issues/2121)
- [Google Cloud Messaging is disabled](https://github.com/brave/brave-browser/issues/1716)
- [Firebase Cloud Messaging is disabled](https://github.com/brave/brave-core/pull/908)
- [Push client channel updates are disabled](https://github.com/brave/brave-core/pull/912)
- [Network time tracker is disabled](https://github.com/brave/brave-core/pull/792)
- [Google-assisted address normalization is disabled](https://github.com/brave/brave-core/pull/769)
- [RAPPOR log uploading is disabled](https://github.com/brave/brave-core/pull/963)
- [Specific features are disabled on startup via the CLI](https://github.com/brave/brave-core/blob/master/app/brave_main_delegate.cc) (search for `disabled_features`)
- [Remove dl.google.com repository from Linux packages](https://github.com/brave/brave-core/pull/1078)
- [Disable metrics reporting](https://github.com/brave/brave-core/pull/2029)
- [Disable field trials and variations service](https://github.com/brave/brave-browser/issues/4283)
- [Disable Lookalike URLs Navigation Suggestions](https://github.com/brave/brave-core/pull/2382/files)
- [Disable Reporting Observers and Reporting API](https://github.com/brave/brave-core/pull/4578)
- [Disable Scroll To Text Fragment](https://github.com/brave/brave-core/pull/4548/commits/3221538c3b2939d11a3074be3d5c8f44b2540a6c)

### Services We Proxy Through Brave Servers

_Google does not receive any information about which client is performing these requests (not even your IP address)._

- [SafeBrowsing requests are proxied](https://github.com/brave/brave-core/pull/108)
- [Geolocation requests are proxied](https://github.com/brave/brave-core/pull/19)
- [Plugin updates are proxied](https://github.com/brave/brave-core/pull/680)
- [Certificate revocation requests are proxied](https://github.com/brave/brave-core/pull/997)
- [Requests for CRLSets are proxied](https://github.com/brave/brave-core/pull/1581)
- [Requests for component updates are proxied](https://github.com/brave/brave-core/pull/1581)
- [Requests for spellcheck dictionaries are proxied](https://github.com/brave/brave-core/pull/2354)

#### Proxied endpoints
- `https://dl.google.com/release2/chrome_component/*crl-set*`
- `https://*.gvt1.com/edgedl/release2/chrome_component/*`
- `https://*.gvt1.com/edgedl/chrome/dict/*.bdic`
- `https://storage.googleapis.com/update-delta/hfnkpimlhhgieaddgfemjhofmfblmnib/.+crxd`
- `https://safebrowsing.googleapis.com/`
- `https://sb-ssl.google.com/`
- `https://ssl.gstatic.com`
- `https://static1.brave.com`
- `https://componentupdater.brave.com`
- `https://clients2.googleusercontent.com`
- `https://clients2.google.com`
- `https://clients4.google.com`
- `https://chrome-devtools-frontend.appspot.com`
- `https://*.infura.io`
- `https://*.gvt1.com/edgel/chromewebstore/*/*`
- `https://*.gvt1.com/edgedl/release2/*/*`
- `http://dl.google.com/release2/*/*`
- `https://translate.googleapis.com/translate_a/element.js*`
- `https://translate.googleapis.com/element/*/js/element/element_main.js`
- `https://translate.googleapis.com/translate_static/js/element/main.js`
- `https://translate.googleapis.com/translate_static/css/translateelement.css`
- `https://www.gstatic.com/images/branding/product/*x/translate_24dp.png`

### Modified Features and Functionality

- Cookies are given a [maximum lifetime](https://github.com/brave/brave-browser/issues/3443) of 7 days for cookies set through Javascript and 6 months for cookies set through HTTP
- Referrer values are modified in the following ways:
  * *cross-site* requests for iframes and sub-resources have their referrer set to be the origin of the requested resource
  * *cross-site* navigations have no referrer at all
  * *same-site* requests of all kinds have the same behavior as Chromium
- Media Router Extension (Chromecast) is disabled by default. You can turn it on by toggling the switch in brave://settings.
- Plugins, sensors and background-sync page permissions are disabled by default. Users can override these using the usual page permission settings.
- Download protection remote lookups are currently disabled (https://github.com/brave/brave-browser/issues/4341).
- Remote debugging is disabled by default. Can be toggled on brave://settings/privacy (implemented with https://github.com/brave/brave-core/pull/4044). This will hit Google servers and is not proxied (yet). There is an [issue created for proxying and enabling this by default](https://github.com/brave/brave-browser/issues/7645). 

### Comments

Some of the above (along with other issues) were previously tracked in https://github.com/brave/brave-browser/issues/13.

You may notice some requests to Google domains. Some of these, such as `clients*.google.com` and `update.googleapis.com` are needed to check for extension updates if you installed extensions.

## How does Brave compare to `ungoogled-chromium`?
Description of [`ungoogled-chromium`](https://github.com/Eloston/ungoogled-chromium), per their GitHub page:
> *ungoogled-chromium is Google Chromium*, sans integration with Google. It also features some tweaks to enhance privacy, control, and transparency _(almost all of which require manual activation or enabling)_.

We have [an issue captured](https://github.com/brave/brave-browser/issues/1431) for pulling in relevant patches from the `ungoogled-chromium` project. The `ungoogled-chromium` project similarly has an [issue captured](https://github.com/Eloston/ungoogled-chromium/issues/543) where they mention pulling in patches from Brave.
