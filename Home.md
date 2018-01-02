Everything you need to build Brave.

## Building
Please ensure you have node.js >= 7.x and Xcode (on macOS) installed on your machine. On macOS, you can install Xcode by running:
```
xcode-select --install
```

You can then get the latest code and build everything.
```bash
git clone git@github.com:brave/brave.git
cd brave
yarn install

# this takes 30-45 minutes to run
# the Chromium source is downloaded which has a large history
yarn run init

# Linux specific steps

./src/build/install-build-deps.sh
apt-get install libgnome-keyring-dev build-essential rpm ninja-build
https://chromium.googlesource.com/chromium/src/+/lkcr/docs/linux_build_instructions.md#notes

# start the compile
yarn build
```
Running a release build with `yarn build` can be very slow and use a lot of RAM especially on Linux with the Gold LLVM plugin.  To speed things up we recommend doing a build with debug symbols and without being an official build.  Instead you'd run `yarn build --debug_build=true --official_build=false`.  For incremental builds also pass `--no_branding_update` to build less files.

## Starting the build

`yarn start`

## Staying up to date

You can run `yarn run sync --all` to grab the latest source. **It's important to note that this will overwrite your local changes, so please back up work before running this**. This typically triggers a full rebuild. If you'd prefer to manually update, you can re-run the antimuon patches by running `yarn run sync --run_hooks`

### Troubleshooting 

#### Troubleshooting on MacOS

As of Chromium 62, the 10.12 SDK is required for building.

`git clone git@github.com:phracker/MacOSX-SDKs.git`

And then before building make a symbolic link to MacOSX10.12.sdk in `Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs`
