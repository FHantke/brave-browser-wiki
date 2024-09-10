# Prerequisites

You will need the prerequisites below to build Brave on macOS 10.15+.

macOS SDK 14.0 is needed as per https://source.chromium.org/chromium/chromium/src/+/main:build/config/mac/mac_sdk.gni;l=43. This is bundled with Xcode 15.0 for example as per https://xcodereleases.com

There are additional details in the Chromium build system requirements at https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements (do not follow any of the instructions after system requirements):

- Node.js active LTS version (v20) - `brew install nvm` and `nvm install --lts && nvm alias default lts/* && nvm use default`

- Python - 2.7 and 3+ - **Note:** On macOS 12.3 and higher Python 2.7 is no longer included by default and you will have to [install it manually](https://www.python.org/downloads/release/python-2718/) or `brew install python3`

# Other tips

- Make sure you have Spotlight fully off or privacy on for the build directory (or indexing will slow it down). System Preferences -> Spotlight -> Privacy

# Build Acceleration

Internal developers can find more information on remote build execution [here](https://github.com/brave/devops/wiki/Remote-Build-Execution)

# Build Configuration

Please refer to [this document](https://github.com/brave/brave-browser/wiki/Build-configuration) for information about setting up `.env` build configuration.
