# Prerequisites

You will need the following prerequisites to build Brave on macOS 10.15+:

- Xcode 11.3+
    - You can install Xcode through the App Store, or by running `xcode-select --install`.
    - There are additional details in the Chromium build system requirements. Do not follow any of the instructions after system requirements https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements
- NodeJS (LTS Version - currently 12.x)
- npm (or [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable))
- Git >= 2.17.0
  - Git is included with Xcode's "Command Line Tools," but the version provided by Apple is old and sometimes has problems interoperating with current Git stable, e.g. when generating diffs using the default settings. You should install a newer version of Git, e.g. via Homebrew.

> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

# Installing macOS SDK

You'll need at least version 10.15.1 of the macOS SDK from https://github.com/phracker/MacOSX-SDKs.

For example:
```
cd ~/Documents
git clone git@github.com:phracker/MacOSX-SDKs.git
sudo ln -s ~/Documents/MacOSX-SDKs/MacOSX10.15.sdk /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs
```