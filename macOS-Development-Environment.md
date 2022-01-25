# Prerequisites

You will need the prerequisites below to build Brave on macOS 10.15+.

Xcode 13.1+ and macOS SDK 12.0 are needed for official builds on Chromium 98. Chromium 99 requires Xcode 13.2+ and macOS SDK 12.1.
Xcode 12.5+ can be used in development for now with component builds. See https://source.chromium.org/chromium/chromium/src/+/main:build/config/mac/mac_sdk.gni;l=43

You'll want an SDK setup like this:
```
ls -la /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs
```

```
MacOSX.sdk - linked to what XCode comes with
MacOSX11.3.sdk
MacOSX12.0.sdk
```

You can get the newer SDKs from https://ci.brave.com/userContent/ (VPN required)

There are additional details in the Chromium build system requirements at https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements (do not follow any of the instructions after system requirements):

- NodeJS LTS Version (`brew install node`) - can also use `nvm`
> If `node --version` fails when installing `node@12` through Homebrew or reports a different version, run `brew link --force --overwrite node`
- npm (or [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable))
> npm is automatically installed if you install NodeJS through Homebrew
> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

# Other tips

- Make sure you have Spotlight fully off or privacy on for the build directory (or indexing will slow it down). System Preferences -> Spotlight -> Privacy
