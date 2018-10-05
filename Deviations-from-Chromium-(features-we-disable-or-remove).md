_**NOTE: this page is a work in progress! It should by no means be considered a "final" or exhaustive list of things we have removed.**_

--------
- [How it works](#how-it-works)
- [What Chromium features are removed for privacy/security reasons?](#what-chromium-features-are-removed-for-privacysecurity-reasons)
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

### Hooks are ran
After the gclient sync runs and fetches all the code (including `brave-core`), the hooks are ran. One of the hooks that runs applies the patches ([which you can see here](https://github.com/brave/brave-core/tree/master/patches)) that are contained in `brave-core`. If you'd like to know more details about HOW the patching works, you can [take a peek at our patching wiki page](https://github.com/brave/brave-browser/wiki/Patching-Chromium)


## What Chromium features are removed for privacy/security reasons?

### Specific issues
- Disabling domain service reliability: https://github.com/brave/brave-core/pull/246
- Disable Chrome Google URL Tracker: https://github.com/brave/brave-core/pull/248
- Disable google services in privacy settings: https://github.com/brave/brave-core/pull/244
- <abbr title="Google Accounts and ID Administration">GAIA</abbr> URLs are set to `no-thanks.invalid` in brave-core: https://github.com/brave/brave-core/pull/512
- Disable DNS prefetching: https://github.com/brave/brave-core/pull/340
- Disable chrome.webstore.install for inline extensions: https://github.com/brave/brave-browser/issues/614
- Disable background sync: https://github.com/brave/brave-browser/issues/515
- Disable google accounts integration and chrome sync
- SafeBrowsing requests are proxied https://github.com/brave/brave-core/pull/108

### Comments
Some of the above (along with other issues) were tracked in https://github.com/brave/brave-browser/issues/13.

You may notice some requests to Google domains. Some of these, such as `clients*.google.com` and `update.googleapis.com` are needed to check for extension updates, such as for PDFJS (which is our built-in PDF reader).

Google translate is disabled as of this commit: https://github.com/brave/brave-core/commit/3351db79213b04d0879b9e284c4914c3da214a16

<abbr title="Google Accounts and ID Administration">GAIA</abbr> (Google's sign-in code) is effectively disabled by setting the service URL to an invalid endpoint: https://github.com/brave/brave-browser/issues/1312

## How does Brave compare to `ungoogled-chromium`?
Description of [`ungoogled-chromium`](https://github.com/Eloston/ungoogled-chromium), per their GitHub page:
> *ungoogled-chromium is Google Chromium*, sans integration with Google. It also features some tweaks to enhance privacy, control, and transparency _(almost all of which require manual activation or enabling)_.

We have [an issue captured](https://github.com/brave/brave-browser/issues/1431) for pulling in relevant patches from the `ungoogled-chromium` project. The `ungoogled-chromium` project similarly has an [issue captured](https://github.com/Eloston/ungoogled-chromium/issues/543) where they mention pulling in patches from Brave.
