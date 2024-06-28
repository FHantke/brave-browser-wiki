# System Requirements

Before you begin, ensure your system satisfies the [system requirements](https://chromium.googlesource.com/chromium/src/+/master/docs/linux/build_instructions.md#system-requirements).

# Install additional build dependencies

You will need Git, Python 3 and Node.js active LTS (v20+). You may need to make `python3` the default if Python 2.7 is default for your OS. Also, if you don't have anything named `python` on your machine and only have `python3`, you will need something like `python-is-python3`.

> Alternatively to npm, you can also use Yarn. We recommend following the [Yarn install docs](https://yarnpkg.com/lang/en/docs/install/) to install Yarn and a compatible version of Node.js. After installing `yarn` you'll want to run `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

If you are using Ubuntu, additionally install:

```
apt-get install build-essential python-setuptools python3-distutils
```

You are now ready to [[clone and initialize the repo|Home#clone-and-initialize-the-repo]]. After `npm run init` is finished, there is one final step to finish installing build dependencies. This shell script only works on Debian and Ubuntu but check [system requirements](https://github.com/chromium/chromium/blob/master/docs/linux/build_instructions.md#system-requirements) for other distros: 

```
# cd to brave-browser repo root
./src/build/install-build-deps.sh # for Linux
```

You might also want to try `./src/build/install-build-deps.sh --unsupported` if above command gives an error about using a non supported Linux distribution. 

Use `./src/build/install-build-deps.sh --android` for Android builds.

# Build Acceleration

Internal developers can find more information on remote build execution [here](https://github.com/brave/devops/wiki/Remote-Build-Execution)

# Build Configuration

Please refer to [this document](https://github.com/brave/brave-browser/wiki/Build-configuration) for information about setting up `.env` build configuration.

# Troubleshooting

Check out the upstream [Checking out and building Chromium on Linux](https://chromium.googlesource.com/chromium/src/+/main/docs/linux/build_instructions.md) docs before filing an issue.

## Known issues
On debug build for Android lint may crash with error `java.lang.OutOfMemoryError: Java heap space`. Solution is to set environment variable `export JAVA_OPTS="-Xmx10G -Xms1G"`.

# Making debug build for Android

In order to make sure output format is apk, add `--target_android_output_format=apk` to build command:
```
npm run build -- Debug --target_os=android --target_arch=arm --target_android_output_format=apk
```

To change target SDK level, add `--target_android_base`, e.g.:
```
npm run build -- Debug --target_os=android --target_arch=arm --target_android_output_format=apk --target_android_base=mono
```

# Installing a build on Android

Both for devices and if you have a started emulator:

` ./src/build/android/adb_install_apk.py ./src/out/android_Debug_x86/apks/Bravex86.apk `

Or

` adb install ./src/out/android_Debug_x86/apks/Bravex86.apk `

If you have an aab file:
```
bundletool build-apks --connected-device --bundle=out/android_Debug_x86/apks/Bravex86.aab --output=out/android_Debug_x86/apks/Bravex86.apks
bundletool install-apks --apks=out/android_Debug_x86/apks/Bravex86.apks
```

# Getting crash dumps for Android

`adb logcat -d | third_party/android_platform/development/scripts/stack --output-directory out/android_Component_arm`

# Other debugging instructions for Android

https://chromium.googlesource.com/chromium/src/+/master/docs/android_debugging_instructions.md
