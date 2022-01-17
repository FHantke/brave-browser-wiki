# Prerequisites

You will need the prerequisites below to build Brave on macOS 10.15+.

Xcode 13.1 or higher is recommended and needed for official builds.
Xcode 12.5 can be used in development for now with component builds.

There are additional details in the Chromium build system requirements at https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements (do not follow any of the instructions after system requirements):

- NodeJS LTS Version (`brew install node`) - can also use `nvm`
> If `node --version` fails when installing `node@12` through Homebrew or reports a different version, run `brew link --force --overwrite node`
- npm (or [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable))
> npm is automatically installed if you install NodeJS through Homebrew
> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

# Other tips

- Make sure you have Spotlight fully off or privacy on for the build directory (or indexing will slow it down). System Preferences -> Spotlight -> Privacy
