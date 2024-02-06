# Prerequisites

1. First follow the [macOS Development Environment](https://github.com/brave/brave-browser/wiki/macOS-Development-Environment) prerequisites.

2. If you haven't already cloned brave-core, please follow the instructions in [brave-browser's README](https://github.com/brave/brave-browser/blob/master/README.md) to do so. Note: After cloning and running init you may need to have `depot_tools` in your PATH (e.g. `$ export PATH="$PATH:/path/to/src/brave/vendor/depot_tools"`)

Note:

The iOS build used to be found in a separate [brave-ios](https://github.com/brave/brave-ios) repo. This is now not the case, and you will find the same code in `src/brave/ios/brave-ios`. **Only Swift code that is built with Xcode should be added to this folder**. All other code should use GN and exist in the appropriate folders outside of `brave-ios`.

# Building

Xcode will handle building `brave-core` for you based on the scheme and target you have picked. This means that if you build Debug to the iOS simulator, it will automatically build a Debug simulator build based on your host machines architecture (arm/intel). Note that this does mean that switching to a device build may incur a large build time if you haven't done so prior. 

Note: Xcode builds brave-core using a **scheme pre-action**, which unfortunately does not show any progress indicator when you start the build aside from showing the "Stop" button. You can monitor progress from the build log until the pre-action completes.

Alternatively, if you'd like you can still build brave-core yourself using the standard `npm run build` commands and Xcode will use the output.

### Building without brave-core

If you are doing frontend only work or are making frequent small incremental fixes in the Xcode project only it may be annoying eating the cost of building core each time even when there are no changes. You can avoid this by:

1. Ensuring you have already built the Xcode project _at least once_ since last pulling master and running sync _using the standard Debug/Release scheme_. This will make sure BraveCore `xcframeworks` are prepared already and ready to use.
2. Creating a duplicate Debug scheme that does not have a pre-action. (Expand "Build" in the scheme, select "Pre-Action", then tap the trash button on the run script phase).

You can then use this scheme locally as you need, but keep in mind to use the Debug build often to keep everything in sync.

### Debugging

Breakpoints should work like normal across both brave-core and brave-ios codebases as long as you ran the initial bootstrap script in the prerequisites list which will create an `LLDBInit` file to fix debug source mapping.

### Build Acceleration

Internal developers can find more information on remote build execution [here](https://github.com/brave/devops/wiki/Remote-Build-Execution)

### Build Configuration

Please refer to [this document](https://github.com/brave/brave-browser/wiki/Build-configuration) for information about setting up `.env` build configuration.