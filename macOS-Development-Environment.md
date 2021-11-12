# Prerequisites

You will need the prerequisites below to build Brave on macOS 10.15+.

There are additional details in the Chromium build system requirements at https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements (do not follow any of the instructions after system requirements):

- NodeJS LTS Version - currently 12.x (`brew install node@12`) - can also use `nvm`
> If `node --version` fails when installing `node@12` through Homebrew or reports a different version, run `brew link --force --overwrite node@12`
- npm 6.x (or [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable))
> npm is automatically installed if you install NodeJS through Homebrew. Please ensure that you're really using npm 6.x by running `npm --version`, npm 7 (or later) does not work.
> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

# Other tips

- Make sure you have Spotlight fully off or privacy on for the build directory (or indexing will slow it down). System Preferences -> Spotlight -> Privacy
- If you see `Connection to github.com closed by remote host` adding the following lines to `~/.ssh/config`:

        ServerAliveInterval 60
        ServerAliveCountMax 100
