Here are some tips for developing Brave. Add yours here as well!

## Coding style

`brave-core` follows the [Chromium style guide](https://google.github.io/styleguide/cppguide.html). In short:
```
 - variable_names
 - member_variable_names_
 - FunctionNames()
 - kEnumValues
 - MACRO_NAMES
 - ClassNames
```

## Header files
For each function used in a .cc file, add the necessary .h file, but don’t add any additional .h files. If a .h file is needed to avoid an incorrect re-definition in a `chromium_src` override file, then add a comment to that effect. https://google.github.io/styleguide/cppguide.html#Include_What_You_Use

## `chromium_src` overrides
Minimize the length of `#define` definitions by factoring out functions or methods wherever possible.

## Code review

Don’t squash commits after reviews. That way your review can see intermediate fixes. Only squash before merging.

## Android

Building and running Android seems to only work on Linux (not macOS) and so far only on x86 (not x64). Use Android Studio > Device Manager > Create Device. When creating a device, choose an x86 image (not an x86_64).

Then use 
```
npm run build -- --target_os=android --target_arch=x86 --target_android_output_format=apk
```

```
adb install ./src/out/android_Component_x86/apks/Bravex86.apk
```

## Running unit tests and browser tests
To run a browser test, you can use
```
npm run test -- brave_browser_tests --filter="*BraveScreenFarblingBrowserTest*"
```
to run an Android test, instead use
```
npm run test -- brave_unit_tests --target_os=android --target_arch=x86 --manual_android_test_device
npm run test -- brave_browser_tests --target_os=android --target_arch=x86 --manual_android_test_device
```
