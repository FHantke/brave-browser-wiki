# Prerequisites

You will need the prerequisites below to build Brave on macOS 10.15+. There are additional details in the Chromium build system requirements at https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements (do not follow any of the instructions after system requirements)

- Xcode 12.2+ (can install through the App Store, or by running `xcode-select --install`)
- MacOS SDK 11.1(or from https://github.com/phracker/MacOSX-SDKs)
- NodeJS LTS Version - currently 12.x (`brew install node@12`) - can also use `nvm`
> If `node --version` fails when installing `node@12` through Homebrew or reports a different version, run `brew link --force --overwrite node@12`
- npm 6.x (or [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable))
> npm is automatically installed if you install NodeJS through Homebrew. Please ensure that you're really using npm 6.x by running `npm --version`, npm 7 (or later) does not work.
- Git >= 2.17.0
  - Git is included with Xcode's "Command Line Tools," but the version provided by Apple is old and sometimes has problems interoperating with current Git stable, e.g. when generating diffs using the default settings. You should install a newer version of Git, e.g. via Homebrew.

> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

# Other tips

- Make sure you have spotlight privacy on for the build directory or indexing will slow it down. System Preferences -> Spotlight -> Privacy