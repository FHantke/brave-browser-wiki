# System Requirements

Before you begin, ensure your system satisfies the [system requirements](https://chromium.googlesource.com/chromium/src/+/master/docs/linux/build_instructions.md#system-requirements).

# Install additional build dependencies

You will need Git, Python 2.7, the LTS version NodeJS, and npm.

> Alternatively to npm, you can also use Yarn. We recommend following the [Yarn install docs](https://yarnpkg.com/lang/en/docs/install/) to install Yarn and a compatible version of NodeJS. After installing `yarn` you'll want to run `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

If you are using Ubuntu, additionally install:

```
apt-get install build-essential libgnome-keyring-dev python-setuptools npm
```

You are now ready to [[clone and initialize the repo|Home#clone-and-initialize-the-repo]]. After `npm run init` is finished, there is one final step to finish installing build dependencies. This shell script only works on Debian and Ubuntu but check [system requirements](https://github.com/chromium/chromium/blob/master/docs/linux/build_instructions.md#system-requirements) for other distros: 

```
# cd to brave-browser repo root
./src/build/install-build-deps.sh
```
You might also want to try `./src/build/install-build-deps.sh --unsupported` if above command gives an error about using a non supported Linux distribution. 

Use `./src/build/install-build-deps-android.sh` for android builds

# Troubleshooting

Check out the upstream [Checking out and building Chromium on Linux](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md) docs before filing an issue.

## Known issues
On debug build for Android lint may crash with error `java.lang.OutOfMemoryError: Java heap space`. Solution is to set environment variable `export JAVA_OPTS="-Xmx10G -Xms10G"`.

# Making debug build for Android
If you are building debug apk for Android and doing debugging, you may want to add `--gn=enable_proguard_obfuscation:false` to build command:
```
npm run build -- --target_os=android --target_arch=arm --gn=enable_proguard_obfuscation:false
```
This will give you nice stacks from Java crashes or exceptions without need to run retrace script.

# Installing a build on Android

Both for devices and if you have a started emulator:

` ./src/build/android/adb_install_apk.py ./src/out/android_Debug_x86/apks/Bravex86.apk `