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

```
# start the compile
yarn build
```

Running a release build with `yarn build` can be very slow and use a lot of RAM especially on Linux with the Gold LLVM plugin.  To speed things up we recommend doing a build with debug symbols and without being an official build.  Instead you'd run `yarn build --debug_build=true --official_build=false`.  For incremental builds also pass `--no_branding_update` to build less files.

## Run Brave

`yarn start`

# Staying up to date

You can run `yarn run sync --all` to grab the latest source. **It's important to note that this will overwrite your local changes, so please back up work before running this**. This typically triggers a full rebuild. If you'd prefer to manually update, you can re-run the brave-core patches by running `yarn run sync --run_hooks`