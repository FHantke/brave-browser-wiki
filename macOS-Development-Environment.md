# Prerequisites

You will need the following prerequisites to build Brave on macOS:

- Xcode
    - You can install Xcode through the App Store, or by running `xcode-select --install`.
    - There are additional details in the chromium build system requirements. Do not follow any of the instructions after system requirements https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements
- NodeJS >= 7.x
- npm (or [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable))
- Git >= 2.17.0
  - Git is included with Xcode's "Command Line Tools," but the version provided by Apple is old and sometimes has problems interoperating with current Git stable, e.g. when generating diffs using the default settings. You should install a newer version of Git, e.g. via Homebrew.

> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

# Troubleshooting

As of Chromium 62, the 10.12 SDK is required for building. If you are using macOS < 10.12, you may need to:

`git clone git@github.com:phracker/MacOSX-SDKs.git`

And then before building make a symbolic link to MacOSX10.12.sdk in `Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs`