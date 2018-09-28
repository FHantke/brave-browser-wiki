# Prerequisites

You will need the following prerequisites to build Brave on macOS:

- Xcode
    - You can install Xcode through the App Store, or by running `xcode-select --install`.
    - There are additional details in the chromium build system requirements. Do not follow any of the instructions after system requirements https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements
- NodeJS >= 7.x
- npm (or [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable))
- Git >= 2.17.0
  - Git is included with Xcode's "Command Line Tools," but the version provided by Apple is old and sometimes has problems interoperating with current Git stable, e.g. when generating diffs using the default settings. You should install a newer version of Git, e.g. via Homebrew.

> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

# Troubleshooting

As of Chromium 62, the 10.12 SDK is required for building. If you are using macOS < 10.12, you may need to:

`git clone git@github.com:phracker/MacOSX-SDKs.git`

And then before building make a symbolic link to MacOSX10.12.sdk in `Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs`

For example:
```
cd ~/Documents
git clone git@github.com:phracker/MacOSX-SDKs.git
sudo ln -s ~/Documents/MacOSX-SDKs/MacOSX10.12.sdk /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs
```

If you have 64 <= chromium version <= 70, please just use Xcode 9(10.13 SDK) or you might encounter 
```
gen/components/autofill/content/common/autofill_agent.mojom-shared.h:394:16: error: too few arguments provided to function-like macro invocation
  bool require() const {
               ^
../../../../../../../Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.12.sdk/usr/include/AssertMacros.h:1307:11: note: macro 'require' defined here
                #define require(assertion, exceptionLabel)  __Require(assertion, exceptionLabel)
                        ^
In file included from ../../electron/brave/renderer/brave_content_renderer_client.cc:23:
In file included from ../../components/autofill/content/renderer/autofill_agent.h:16:
In file included from gen/components/autofill/content/common/autofill_agent.mojom.h:37:
gen/components/autofill/content/common/autofill_agent.mojom-shared.h:394:15: error: expected ';' at end of declaration list
  bool require() const {
              ^
              ;
gen/components/autofill/content/common/autofill_agent.mojom-shared.h:391:9: error: member initializer 'data_' does not name a non-static data member or base class
      : data_(data) {}
        ^~~~~~~~~~~
gen/components/autofill/content/common/autofill_agent.mojom-shared.h:393:34: error: use of undeclared identifier 'data_'
  bool is_null() const { return !data_; }
                                 ^
4 errors generated.
```
And don't symlink 10.13 SDK in Xcode 10 because you will see
```
********************************************************************************
 WARNING: The Mac OS X SDK is incompatible with the version of Xcode. To fix,
          either upgrade Xcode to the latest version or install the Mac OS X
          10.12 SDK. For more information, see https://crbug.com/620127.

 Current SDK Version:   10.13
 Current Xcode Version: 0100 (10A255)
********************************************************************************
```