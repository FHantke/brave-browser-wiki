Everything you need to build Brave.

# Build Instructions

## Install prerequisites

Follow the instructions for your platform:

- [[macOS|macOS Development Environment]]
- [[Windows|Windows Development Environment]]
- [[Linux|Linux Development Environment]]

## Clone and initialize the repo

Once you have the prerequisites installed, you can get the code and initialize the build environment.

```bash
git clone git@github.com:brave/brave-browser.git
cd brave-browser
yarn install

# this takes 30-45 minutes to run
# the Chromium source is downloaded which has a large history
yarn run init
```

## Build Brave
The default build is debug.
```
# start the debug compile
yarn build
```

To do a release build:
```
# start the release compile
yarn build Release
```
### Speed up the builds

Running a release build with `yarn build Release` can be very slow and use a lot of RAM especially on Linux with the Gold LLVM plugin.  To speed things up we recommend doing a build with debug symbols and without being an official build.  Instead you'd run `yarn build Release --debug_build=true --official_build=false`.

For _incremental_ builds only, pass `--no_branding_update` to skip the branding update and build fewer files. If you see the following error, it means the branding update is required (i.e., you must not pass the flag to skip it):

```
ninja: error: '../../components/components_brave_strings.grd', needed by
'obj/components/strings/components_chromium_strings_grit.inputdeps.stamp', missing and no known rule to make it
```

Note: When using npm to build, specify the following command `npm run build -- Release --debug_build=true --official_build=false`

You may also want to try [[using sccache|Using sccache]].

## Run Brave
To start debug build:

`yarn start`

To start release build:

`yarn start Release`
# Staying up to date

You can run `yarn run sync --all` to grab the latest source. **It's important to note that this will overwrite your local changes, so please back up work before running this**. This typically triggers a full rebuild. If you'd prefer to manually update, you can re-run the brave-core patches by running `yarn run sync --run_hooks`.