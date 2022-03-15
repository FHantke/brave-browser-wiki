# Prerequisites

You will need the prerequisites below to build Brave on macOS 10.15+.

Xcode 13.1+ and macOS SDK 12.0 are needed for official builds on Chromium 98. Chromium 99 requires Xcode 13.2+ and macOS SDK 12.1.
Xcode 12.5+ can be used in development for now with component builds. See https://source.chromium.org/chromium/chromium/src/+/main:build/config/mac/mac_sdk.gni;l=43

You'll want an SDK setup like this:

```
ls -al `xcode-select -p`/Platforms/MacOSX.platform/Developer/SDKs
drwxr-xr-x  7 jenkins  staff  224 Dec 21 12:02 MacOSX.sdk
drwxr-xr-x  7 jenkins  staff  224 May  3  2021 MacOSX11.3.sdk
lrwxr-xr-x  1 jenkins  staff   10 Dec 21 12:02 MacOSX12.0.sdk -> MacOSX.sdk
```

Note: this SDK version information could be out of date, please see [the source code here to check if it points to a higher version](https://source.chromium.org/chromium/chromium/src/+/main:build/config/mac/mac_sdk.gni;l=43)!

You can get the newer SDKs from https://ci.brave.com/userContent/ (VPN required)

There are additional details in the Chromium build system requirements at https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements (do not follow any of the instructions after system requirements):

- Node.js active LTS version (v16+) - `brew install node` or can also use `nvm`. If `node --version` fails when installing `node@16` through Homebrew or reports a different version, run `brew link --force --overwrite node`
- npm - v8+ (or [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable)). npm is automatically installed if you install NodeJS through Homebrew. If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.
- Python - 2.7 and 3+ - **Note:** On macOS 12.3 and higher Python 2.7 is no longer included by default and you will have to [install it manually](https://www.python.org/downloads/release/python-2718/)

# Other tips

- Make sure you have Spotlight fully off or privacy on for the build directory (or indexing will slow it down). System Preferences -> Spotlight -> Privacy
