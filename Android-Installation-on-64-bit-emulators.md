# Why this page is created
Chromium packs both 32 and 64 bit libraries inside 64 bit `apks`. And emulators fail to run them because doesn't handle 32 bit libraries correctly. So to install Brave application on such emulators we should install `aab` instead.

# Instructions how to run to install `aab`
## On Ubuntu
### Prerequisites
Chromium provides all the necessary tools to install `aab` with the source code.

### Installation
From the `src` folder, run these commands:
1. `./build/android/gyp/bundletool.py build-apks --connected-device --bundle=<path to aab file> --output=<path to apks file> --adb=third_party/android_sdk/public/platform-tools/adb`
2. `./build/android/gyp/bundletool.py install-apks --apks=<path to apks file> --adb=third_party/android_sdk/public/platform-tools/adb`

Usually `<path to apks file>` is the same as `<path to aab file>` but with extension `apks`.

Example of the command: `./build/android/gyp/bundletool.py build-apks --connected-device --bundle=out/android_Debug/apks/BraveMonox64.aab --output=out/android_Debug/apks/BraveMonox64.apks --adb=third_party/android_sdk/public/platform-tools/adb`

## On Mac OS
### Prerequisites
1. Install Homebrew from https://brew.sh
2. Run command `brew install android-platform-tools`
3. Run command  `brew install bundletool`

### Installation
Run these commands:
1. `bundletool build-apks --bundle=<path to aab file> --output=<path to apks file>`
2. `bundletool install-apks --apks=<path to apks file>`

Usually `<path to apks file>` is the same as `<path to aab file>` but with extension `apks`.

Example of the command: `bundletool build-apks --bundle=BraveMonoarm64.aab --output=BraveMonoarm64.apks`

## Important notes
Commands above assume that you have only 1 emulator (and no other devices connected) on the machine. Otherwise you will need to specify `device-id` param to the `bundletool` command.

For instance `bundletool build-apks --bundle=BraveMonoarm64.aab --output=BraveMonoarm64.apks --device-id=emulator-5554`.

## Known issues
There is a problem with `vulkan` on `arm64` emulators on `M1` Mac. It shows annoying crash dialogs when apps starts. After closing those dialogs app works as expected.
List of ids can be found by running `adb devices` command.