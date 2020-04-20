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
brave-core based android builds should use `npm run init -- --target_os=android --target_arch=arm` (or whatever cpu type you want to build for)

You can also set the target_os and target_arch for init and build using
```
npm config set target_os android
npm config set target_arch arm
```

## Build Brave
The default build is component.
```
# start the component build compile
npm run build
```

To do a release build:
```
# start the release compile
npm run build Release
```

brave-core based android builds should use `npm run build -- --target_os=android --target_arch=arm` or set the npm config variables as specified above for `init`

### Speed up the builds

Running a release build with `npm run build Release` can be very slow and use a lot of RAM especially on Linux with the Gold LLVM plugin.

To run a statically linked build (takes longer to build, but starts faster)
```bash
npm run build -- Static
```

To run a debug build (Component build with is_debug=true)
```bash
npm run build -- Debug
```

You may also want to try [[using sccache|sccache-for-faster-builds]].

## Run Brave
To start the build:

`npm start [Release|Component|Static|Debug]`

# Staying up to date

- Run `npm run sync` to update the brave-core ref specified in `package.json`. **It's important to note that this will overwrite your local changes, so please back up work before running this**.
- Run `npm run sync -- --all` to update both brave-core and chromium. **It's important to note that this will overwrite your local changes, so please back up work before running this**.

_See below for more advanced options for branch-switching and dependency updating._

# Sync commands

`npm run sync` will (depending on the below flags):
1. 📥 Update sub-projects (chromium, brave-core) to latest commit of a git ref (e.g. tag or branch)
2. 🤕 Apply patches (only those that require re-applying)
3. 🔄 Update gclient DEPS dependencies, only if a project was updated or explicitly asked to.
4. ⏩ Run hooks (e.g. to perform `npm install` on child projects), only if a project was updated or explicitly asked to.
 has a number of switches which can fit different styles of source code management:

These flags can be combined to fit different workflows

| flag | Description |
|---|---|
|`[no flags]`|updates _brave-core_ to the latest remote commit on the branch specified in brave-browser/package.json (e.g. `master` or `74.0.0.103`). Will re-apply only patches that changed. Will update child dependencies **only if any project needed updating during this script run** <br> **Use this if you want the script to manage keeping you up to date instead of pulling or switching branch manually. **|
|`--all`| updates both _Chromium_ and _brave-core_ to the latest remote commit on the branch specified in brave-browser/package.json (e.g. `master` or `74.0.0.103`). Will re-apply only patches that changed. Will update child dependencies **only if any project needed updating during this script run** <br> **Use this if you want the script to manage keeping you up to date instead of pulling or switching branch manually. **|

### Scenarios

#### Automatically get latest on brave-browser master, brave-core master and chromium
```bash
brave-browser> git checkout master
brave-browser> git pull
brave-browser> npm run sync -- --all
...Updating 2 patches...
...Updating child dependencies...
...Running hooks...
```

#### When you know that DEPS didn't change, but .patch files did (quickest)
```bash
brave-core> git checkout featureB
brave-core> git pull
brave-browser> npm run update_patches
...Applying 2 patches...
brave-browser>
```

# Troubleshooting

See [[Troubleshooting]] for solutions to common problems.