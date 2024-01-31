## Configure and init the new Chromium version

1. Create a new cloned directory and install the necessary node modules.

&emsp;`npm install`

2. Since this is a new clone, you won't have a `src` directory so run `init` to create that.

&emsp;`npm run init`

3. Branch off the latest Chromium rebase branch or master, whichever is applicable.

3. Open `package.json` in `src/brave/` and change the string at the path `config.projects.chrome.tag` to the new Chromium version, then commit the changes to that file with the comment `Update from Chromium XXX to Chromium YYY.`

4. Run `npm run sync` to make sure that the Chromium repository at `src/` is at the right version, the right internal dependencies are set to the expected versions, and the patches from Brave applied as expected.

5. If all patches apply correctly, simply run

&emsp;`npm run update_patches`

&emsp;then, commit the changes with the comment `Updated patches from Chromium XXX to Chromium YYY.`

&emsp;Typically, however, some patches will fail to apply.

## Fix patches

1. Resolve errors for each patch that didn't apply.

&emsp;The most common situations are: 
 * The file being patched got moved/renamed -> rename the patch accordingly.
 * The content of the file changed so that the lines surrounding the patched in lines don't match -> adjust the patch to fit the new content.
 Once the patch is fixed, apply it, then, update:
 
 ```
  cd src
  git apply -3 brave/patches/patch_name.patch
  git diff <CHROMIUM_VERSION> --src-prefix=a/ --dst-prefix=b/ --full-index path/to/file/being/patched > brave/patches/patch_name.patch
 ```

 &emsp;When all such failing patches have been fixed, commit the changed patches with the comment `Conflict-resolved patches from Chromium XXX to Chromium YYY.`

 * The content that was being patched or the entire file was removed -> determine if the patch can just be removed or other changes need to made to compensate for the removed code. Remove the patch and commit. The commit message should include the links to the relevant upstream changes on https://chromium.googlesource.com/ or https://source.chromium.org/, as well as the upstream commit messages for those changes.

2. Once all patching errors have been resolved, run update:

&emsp;`npm run update_patches`

&emsp;This will update patches that applied correctly in the first place. Commit the changes with the comment `Updated patches from Chromium XXX to Chromium YYY.`

3. Since the `init` was interrupted by patching errors run `sync`:

&emsp;`npm run sync`

**Note:** Be careful when applying `tools/gritsettings/resource_ids`, sometimes the IDs can change and you should update the Brave one to be the same as the Chromium one.


##  Update strings

Update `grd` and `grdp` files from the Chromium ones:

```
cd ..
npm run chromium_rebase_l10n
```
```
Note: Filled some custom strings to brave_strings.grd by https://github.com/brave/brave-core/pull/214/commits/256f5aa8e781d266ad23f863ac7b613615ad2a5a
```

Inspect changed strings visually to catch overly aggressive replacements (e.g. Google Lens -> Brave Lens).
If overly aggressive replacements are found, add them to `script/lib/l10n/grd_string_replacements.py` and `script/lib/l10n/transifex/push.py` and rerun the `chromium_rebase_l10n` script.

Do a commit for the updated source strings, `grd` files with the comment `Updated strings for Chromium XXX`.

## Check for changes in the build tool chain
* For Mac, check `mac_sdk_min` in `src/build/config/mac/mac_sdk_overrides.gni` for the SDK version, the comment above `MAC_BINARIES_TAG` and `MAC_MINIMUM_OS_VERSION` in `src/build/mac_toolchain.py` for the XCode version and the OS version,
* For Windows check `MSVS_VERSIONS` in `src/build/vs_toolchain.py` (`CURRENT_DEFAULT_TOOLCHAIN_VERSION` in `src/build/win_toolchain.py` prior to C75) for Visual Studio versions.

Update your tool chain if needed and alert dev and CI teams on tool chain changes. 

## Fix the build

Do a build like normal, fix errors as they come up.

`npm run build`

If there's anything that should be called out as a non-trivial change, you should do it as a separate commit.

For each error fixed, the commit message should include the links to the relevant upstream changes on https://chromium.googlesource.com/ or https://source.chromium.org/, as well as the upstream commit messages for those changes.

If you have more fixes caused by the same upstream change, use git commit --fixup and rebase -i to squash them.

```
git log
1234567 Fix for upstream change A.
8910112 Fix for upstream change B.
```

```
git commit -a --fixup 1234567 
git rebase -i --autosquash @~3
```

## Fix chromium_src overrides

Run `brave/tools/chromium_src/check_chromium_src.py` to find faulty overrides in the `chromium_src` directory, including:

* targets under `src` that no longer exist
* definitions for symbols that are no longer present in the target file

Make an appropriate fix for each situation.

## Fix unit and browser tests build

Build unit and browser tests as usual:

`npm run test -- brave_unit_tests`

`npm run test -- brave_browser_tests`

Use the same commit practices as for fixing the main build.

***
***MIDL issue on Windows***

When this message is visible during the build, check `src/third_party/win_build_output/` is changed. If not, it means our pre-generated files in `./src/brave/win_build_output/midl/google_update/` are not copied there. In this case, check `updateOmahaMidlFiles()` in `lib/util.js`. Make sure that `google_update` directory has two directory(x64, x86).

```
To rebaseline:
  copy /y c:\users\XXX\appdata\local\temp\tmpdjhho_\* C:\Projects\brave\brave-browser\src\third_party\win_build_output\midl\google_update\x64
```

## Audit for new network services

When submitting your PR add the `CI/run-network-audit` label to ensure that CI runs the `audit-network` pipeline.

For now just follow the instructions in https://github.com/brave/brave-browser/wiki/Tests#privacy-network-audit.

We should build some automation to find new things added here:

https://cs.chromium.org/search/?q=%22destination:+GOOGLE_OWNED_SERVICE%22&sq=package:chromium&type=cs

## Check for changes in supported OS versions
- [ ] For MacOS check `mac_deployment_target` and `mac_min_system_version` in `build/config/mac/mac_sdk.gni`
- [ ] For iOS check `ios_deployment_target` in `build/config/ios/ios_sdk_overrides.gni`

## Notify Security team of feature changes

- [ ] Check with the Security team in regards to any possibly relevant changes in features or their default settings. Specifically, post link to changes in https://chromium.googlesource.com/chromium/src/+diff/[PreviousVersion]..[NewVersion]/content/public/common/content_features.cc
in #security-discussion Slack channel. 

- [ ] Besides new settings, we'll want to look at brave://components and see if there are new components registered. For example, there are components we have either missed (when added) or we never triaged captured in [this GitHub issue](https://github.com/brave/brave-browser/issues/11544). There's an issue created to automate this process which you [can find here](https://github.com/brave/brave-browser/issues/11811).

- [ ] Entitlements to enable new features specific to Chrome are added to `https://source.chromium.org/chromium/chromium/src/+/master:chrome/app/app-entitlements-chrome.plist`. Review the new entitlements for each CR bump when new features are added to Chrome. If a new entitlement is added, it most likely will result in the MacOS application failing to launch due to a signature error.

- [ ] On Android, new permissions can be added in */AndroidManifest.xml or */AndroidManifest.xml.expected. Users often ask questions about new permissions on update. Any new permissions should be reviewed by the @android-team or @security-team before merge.

## Update localizations

See https://github.com/brave/brave-browser/wiki/Strings-and-Localization

Run this to detect new strings and push them to Transifex, it will also push up the translations for those strings automatically.
If you need access talk to devops.

```
npm run push_l10n
```

Run this to pull down new xtb and translation files:

`npm run pull_l10n`

Do a commit for all the string translation updates, `xtb` files.

## Creating the PR

When creating the PR, mark it as `WIP` and put it into draft mode. The title should be `Upgrade from Chromium {n} to Chromium {n+1}` where `n` is the current version number.

The PR must contain all of the following labels:

* `CI/run-audit-deps`
* `CI/run-draft`
* `CI/run-network-audit`
* `CI/run-upstream-tests`

## Smoke test

Before handing off a build to QA, please test the following functionality:

- Ensure that About page shows the expected version number
- Ensure that `brave://settings` is functional and not throwing any console errors (check each section)
- Ensure that clearing browser data on exit works as expected
- Ensure that tabs have the expected shape (it's slightly different from Chrome)
- Ensure that videos play properly on YouTube and that the media device toolbar button works correctly
- Ensure that tab-muting works properly
- Ensure that vertical tabs work as expected (enable by right-clicking any tab and selecting "Use vertical tabs")
- Ensure that enabling Rewards works properly
- Ensure that all elements of the sidebar panel work as expected
- Ensure that creating and using multiple profiles works as expected
- Ensure that creating and using a Guest profile works as expected (reached via Hamburger -> More tools -> Open Guest profile)
- Ensure that private and Tor windows work as expected
- Ensure that a few different web sites load as expected
- Ensure that DevTools loads correctly and works as expected

## QA Builds

Before merging, we must produce standard (non-delta) and delta builds for QA to test.

### Standard (non-delta) builds

#### Creation

Make non-delta builds for QA smoke tests using the following links:
* MacOS (x64): https://ci.brave.com/view/macos/job/test-brave-browser-build-macos-x64
* MacOS (arm64): https://ci.brave.com/view/macos/job/test-brave-browser-build-macos-arm64
* Linux: https://ci.brave.com/view/linux/job/test-brave-browser-build-linux-x64
* Windows (x64): https://ci.brave.com/view/linux/job/test-brave-browser-build-windows-x64
* Windows (x86): https://ci.brave.com/view/linux/job/test-brave-browser-build-windows-x86
* Android: https://ci.brave.com/view/macos/job/test-brave-browser-build-android
* iOS: https://ci.brave.com/view/macos/job/test-brave-browser-build-ios

Settings:
* `CHANNEL` - Set to `nightly` (this is the default)
* `BRANCH` - Set to the appropriate branch name, e.g., `cr100`
* `PREVIOUS_BUILD_NUMBER_DELTA` - Leave blank
* `KEYCHAIN_NAME` (Mac only) - **Must** be set to `release` (this is now the default, but always double-check it as using the wrong setting will result in an app that crashes on startup)

#### Download

The build jobs produce installers that you can retrieve from the following places (you'll need to modify these links slightly, as they include build and version number information; they're provided here only as an example):

##### MacOS installers
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-macos-x64/600/Brave-Browser-Nightly-x64.dmg 
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-macos-arm64/153/Brave-Browser-Nightly-arm64.dmg

##### Linux installers
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-linux-x64/516/brave-browser-nightly_1.37.54_amd64.deb
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-linux-x64/516/brave-browser-nightly-1.37.54-1.x86_64.rpm

##### Windows (x64) installers
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x64/436/setup-x64.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x64/436/brave_installer-x64.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x64/436/BraveBrowserNightlySetup_99_1_37_54.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x64/436/BraveBrowserStandaloneNightlySetup_99_1_37_54.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x64/436/BraveBrowserStandaloneSilentNightlySetup_99_1_37_54.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x64/436/BraveBrowserStandaloneUntaggedNightlySetup_99_1_37_54.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x64/436/BraveBrowserUntaggedNightlySetup_99_1_37_54.exe

##### Windows (x86) installers
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x86/436/setup-x86.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x86/436/brave_installer-x86.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x86/436/BraveBrowserNightlySetup_99_1_37_54.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x86/436/BraveBrowserStandaloneNightlySetup_99_1_37_54.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x86/436/BraveBrowserStandaloneSilentNightlySetup_99_1_37_54.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x86/436/BraveBrowserStandaloneUntaggedNightlySetup_99_1_37_54.exe
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-windows-x86/436/BraveBrowserUntaggedNightlySetup_99_1_37_54.exe

##### Android installers
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-android-variant/67/Bravearm.apk
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-android-variant/68/BraveMonoarm.apk
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-android-variant/69/Bravex86.apk
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-android-variant/70/BraveMonox86.apk
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-android-variant/71/BraveMonoarm64.apk
* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-android-variant/72/BraveMonox64.apk

##### iOS installers

* https://brave-jenkins-build-artifacts.s3.amazonaws.com/test-brave-browser-build-ios/193/brave-core-ios-1.48.51.tgz

### Delta builds

#### Creation

After creating the standard builds, next create the delta builds for Windows and Mac.

Note: You **MUST** first wait for the version number to increment on master and then rebase your branch against master to get that new version, as the delta updater must update to a new version number.

Make delta builds for QA smoke tests using the following links:
* MacOS (x64): https://ci.brave.com/view/macos/job/test-brave-browser-build-macos-x64
* MacOS (arm64): https://ci.brave.com/view/macos/job/test-brave-browser-build-macos-arm64
* Windows (x64): https://ci.brave.com/view/linux/job/test-brave-browser-build-windows-x64
* Windows (x86): https://ci.brave.com/view/linux/job/test-brave-browser-build-windows-x86

Settings (all settings are the same as used above for the non-delta QA builds, except):
* `PREVIOUS_BUILD_NUMBER_DELTA` - Specify the build number from the previous non-delta build that you created above

#### Download

The build jobs produce installers that you can retrieve from the following places (you'll need to modify these links slightly, as they include build and version number information; they're provided here only as an example):

##### MacOS deltas
* https://build-artifacts.brave.com/test-brave-browser-build-macos-x64/1076/Brave-Browser-Nightly-x64.delta (x64)
* https://build-artifacts.brave.com/test-brave-browser-build-macos-arm64/429/Brave-Browser-Nightly-arm64.delta (arm64)

##### Windows deltas
* https://build-artifacts.brave.com/test-brave-browser-build-windows-x64/1136/brave_installer-delta-x64.exe (x64)
* https://build-artifacts.brave.com/test-brave-browser-build-windows-x86/592/brave_installer-delta-ia32.exe (x86)

#### Uploading

After producing the delta builds, you can upload them to the update server here:
https://ci.brave.com/job/test-brave-browser-updates-upload

Settings:
* `JOB` - A job ending with `-windows-[arch]` or `-macos-[arch]`, eg. `test-brave-browser-build-windows-x64`
* `BUILD_NUMBER` - The build number of the delta build
* `OMAHA_CHANNEL` - The channel on the update server to upload the version to

#### Testing

##### Windows

In order to test a delta update on Windows, do the following:

1. Connect to the Brave VPN
1. Download and install the appropriate standalone installer (e.g., `BraveBrowserStandaloneNightlySetup_119_1_61_36.exe`)
1. Once the browser launches:
   - Open brave://version
   - Verify that correct version is installed
   - Close the browser
1. In the Windows registry, set `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\BraveSoftware\UpdateDev\url` to `https://updates.brave.com/service/update2`
1. In the Windows registry, set `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\BraveSoftware\Update\ClientState\{C6CB981E-DB30-4876-8639-109F8933582C}\ap` to the appropriate channel name (e.g., `cr-bump-update-tests-x64`)
1. Launch the browser and visit brave://settings/help
1. Verify that the browser downloads and installs the delta update

## Merge

After QA signoff on the test builds, merge to `master` and announce it on the appropriate Slack channels. At this time, you can also note for developers any process/style/toollkit/etc. changes required by the new Chromium version.

After merging to `master` create uplifts as appropriate.

## Minor bumps

For post-merge minor Chromium bumps, [follow the instructions here](https://github.com/brave/brave-browser/wiki/Brave-Release-Minor-Updates).