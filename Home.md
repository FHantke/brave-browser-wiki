Everything you need to build Brave.

- [Desktop product roadmap](https://github.com/brave/brave-browser/wiki/roadmap)
- [Release information](https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule)

# Build Instructions

## Install prerequisites

Follow the instructions for your platform:

- [[macOS|macOS Development Environment]]
- [[Windows|Windows Development Environment]]
- [[Linux/Android|Linux Development Environment]]

## Clone and initialize the repo

Once you have the prerequisites installed, you can get the code and initialize the build environment.

```bash
git clone git@github.com:brave/brave-browser.git
cd brave-browser
npm install

# this takes 30-45 minutes to run
# the Chromium source is downloaded which has a large history
npm run init
```
brave-core based android builds should use `npm run init -- --target_os=android`

## Build Brave
The default build is debug.
```
# start the debug compile
npm run build
```

To do a release build:
```
# start the release compile
npm run build Release
```

brave-core based android builds should use `npm run build -- --target_os=android` or `npm run build -- --target_os=android Release`

### Speed up the builds

Running a release build with `npm run build Release` can be very slow and use a lot of RAM especially on Linux with the Gold LLVM plugin.  To speed things up we recommend doing a build with debug symbols and without being an official build.  Instead you'd run 

```bash
npm run build -- Release --debug_build=true --official_build=false
```

You may also want to try [[using sccache|sccache-for-faster-builds]].

## Run Brave
To start debug build:

`npm start`

To start release build:

`npm start Release`
# Staying up to date

You can run `npm run sync -- --all` to grab the latest source. **It's important to note that this will overwrite your local changes, so please back up work before running this**. This typically triggers a full rebuild. If you'd prefer to manually update, you can re-run the brave-core patches by running `npm run sync -- --run_hooks`.

# Troubleshooting

See [[Troubleshooting]] for solutions to common problems.