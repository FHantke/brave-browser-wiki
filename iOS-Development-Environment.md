# Prerequisites

1. First follow the [macOS Development Environment](https://github.com/brave/brave-browser/wiki/macOS-Development-Environment) prerequisites. (With one exception: iOS requires Xcode 15.3)

2. If you haven't already cloned brave-core, please follow the instructions in [brave-browser's README](https://github.com/brave/brave-browser/blob/master/README.md) to do so. Note: After cloning and running init you may need to have `depot_tools` in your PATH (e.g. `$ export PATH="$PATH:/path/to/src/brave/vendor/depot_tools"`)

Note:

The iOS build used to be found in a separate [brave-ios](https://github.com/brave/brave-ios) repo. This is now not the case, and you will find the same code in `src/brave/ios/brave-ios`. **Only Swift code that is built with Xcode should be added to this folder**. All other code should use GN and exist in the appropriate folders outside of `brave-ios`.

# Building

Xcode will handle building `brave-core` for you based on the scheme and target you have picked. This means that if you build Debug to the iOS simulator, it will automatically build a Debug simulator build based on your host machines architecture (arm/intel). Note that this does mean that switching to a device build may incur a large build time if you haven't done so prior. 

Note: Xcode builds brave-core using a **scheme pre-action**, which unfortunately does not show any progress indicator when you start the build aside from showing the "Stop" button. You can monitor progress from the build log until the pre-action completes.

Alternatively, if you'd like you can still build brave-core yourself using the standard `npm run build` commands and Xcode will use the output.

### Building without brave-core

If you are doing frontend only work or are making frequent small incremental fixes in the Xcode project only it may be annoying eating the cost of building core each time even when there are no changes. You can avoid this by using the `Debug (No Core)` Xcode scheme. This scheme will only update the symlink based on your selected target device and run a webpack.

You can also use this scheme if you want to apply custom arguments to the build like providing `--offline` or `--force_gn_gen`, but keep in mind that switching from simulator to device and vice-versa you will need to remember to run builds for that particular configuration manually. 

### Debugging

Breakpoints should work like normal across both brave-core and brave-ios codebases. The `ios_bootstrap` script (which runs automatically as part of `sync`) will create an `LLDBInit` file to fix debug source mapping when build acceleration is enabled.

### Build Acceleration

Internal developers can find more information on remote build execution [here](https://github.com/brave/devops/wiki/Remote-Build-Execution)

### Build Configuration

Please refer to [this document](https://github.com/brave/brave-browser/wiki/Build-configuration) for information about setting up `.env` build configuration.

### Xcode Performance with built-in SCM:

Xcode's Source Control feature seems to track changes even in parent git repositories, which means it tracks Chromium's src git repo as well which is very slow. While you can keep the Xcode SCM feature _on_ if you'd like, if you do notice any lag while working within Xcode please turn off the `Text Editing: Show Source Control Changes` preference found in Xcode's preference window (⌘+,) > Source Control.