## Configure and init the new Chromium version.

1. Create a new cloned dir.

&emsp;`npm install`

2. Open `package.json` in the root repo and change the string at the path `config.projects.chrome.dir.tag` to the new Chromium version.

3. Since you’re using a new cloned dir you should not already have an `src` dir and should run `init`.

&emsp;`npm run init`

4. If all patches apply correctly, simply run

&emsp;`npm run update_patches`

&emsp;then, commit the changes with the comment "Updated patches from Chromium XXX to Chromium YYY."

&emsp;Typically, however, some patches will fail to apply.

## Fix patches.

1. Resolve errors for each patch that didn't apply.

&emsp;The most common situations are: 
 * The file being patches got moved/renamed -> rename the patch accordingly.
 * The content of the file changed so that the lines surrounding the patched in lines don't match -> adjust the patch to fit the new content.
 Once the patch is fixed, apply it, then, update:
 
 ```
  cd src
  git apply -3 brave/patches/patch_name.patch
  git diff <CHROMIUM_VERSION> --src-prefix=a/ --dst-prefix=b/ --full-index path/to/file/being/patched > brave/patches/patch_name.patch
 ```

 &emsp;When all such failing patches have been fixed, commit the changed patches with the comment "Conflict-resolved patches from Chromium XXX to Chromium YYY."

 * The content that was being patched or the entire file was removed -> determine if the patch can just be removed or other changes need to made to compensate for the removed code. Remove the patch and commit. The commit message should include the links to the relevant upstream changes on https://chromium.googlesource.com/ or https://source.chromium.org/, as well as the upstream commit messages for those changes.

2. Once all patching error have been resolved, run update:

&emsp;`npm run update_patches`

&emsp;This will update patches that applied correctly in the first place. Commit the changes with the comment "Updated patches from Chromium XXX to Chromium YYY."

3. Since the `init` was interrupted by patching errors run `sync`:

&emsp;`npm run sync`

**Note:** Be careful when applying `tools/gritsettings/resource_ids`, sometimes the IDs can change and you should update the Brave one to be the same as the Chromium one.


###  Fix strings.

Update `grd` and `grdp` files from the Chromium ones:

```
cd ..
npm run chromium_rebase_l10n
```
```
Note: Filled some custom strings to brave_strings.grd by https://github.com/brave/brave-core/pull/214/commits/256f5aa8e781d266ad23f863ac7b613615ad2a5a
```

Do a commit for the updated source strings, `grd` files with the comment "Updated strings for Chromium XXX".

## Check for changes in the build tool chain.
* For Mac, check `mac_sdk_min` in `src/build/config/mac/mac_sdk_overrides.gni` for the SDK version, the comment above `MAC_BINARIES_TAG` and `MAC_MINIMUM_OS_VERSION` in `src/build/mac_toolchain.py` for the XCode version and the OS version,
* For Windows check `MSVS_VERSIONS` in `src/build/vs_toolchain.py` (`CURRENT_DEFAULT_TOOLCHAIN_VERSION` in `src/build/win_toolchain.py` prior to C75) for Visual Studio versions.

Update your tool chain if needed and alert dev and CI teams on tool chain changes. 

## Fix build.

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

Run `brave/script/check_chromium_src.py` to locate any override files in `chromium_src` override directory:

* whose targets under `src` no longer exist
* that contain definitions for symbols that are no longer present in the target file

Make an appropriate fix for each situation.


## Fix unit and browser tests build.

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


# Audit for new network services

When submitting PR on Github add `CI/run-network-audit` label to the PR to ensure that the CI runs `audit-network`.

For now just follow the instructions in https://github.com/brave/brave-browser/wiki/Tests#privacy-network-audit.

We should build some automation to find new things added here:

https://cs.chromium.org/search/?q=%22destination:+GOOGLE_OWNED_SERVICE%22&sq=package:chromium&type=cs

# Check for changes in supported OS versions.
- [ ] For MacOS check `mac_deployment_target` and `mac_min_system_version` in `build/config/mac/mac_sdk.gni`

# Notify Security team of feature changes

- [ ] Check with the Security team in regards to any possibly relevant changes in features or their default settings. Specifically, post link to changes in https://chromium.googlesource.com/chromium/src/+diff/[PreviousVersion]..[NewVersion]/content/public/common/content_features.cc
in #security-discussion Slack channel. 

- [ ] Besides new settings, we'll want to look at brave://components and see if there are new components registered. For example, there are components we have either missed (when added) or we never triaged captured in [this GitHub issue](https://github.com/brave/brave-browser/issues/11544). There's an issue created to automate this process which you [can find here](https://github.com/brave/brave-browser/issues/11811).

- [ ] Entitlements to enable new features specific to Chrome are added to `https://source.chromium.org/chromium/chromium/src/+/master:chrome/app/app-entitlements-chrome.plist`.Review the new entitlements for each CR bump when new features are added to Chrome.

- [ ] On Android, new permissions can be added in */AndroidManifest.xml or */AndroidManifest.xml.expected. Users often ask questions about new permissions on update. Any new permissions should be reviewed by the @android-team or @security-team before merge.

### Update localization.

See https://github.com/brave/brave-browser/wiki/Strings-and-Localization

Run this to detect new strings and push them to Transifex, it will also push up the translations for those strings automatically.
If you need access talk to devops.

```
npm run push_l10n
```

Run this to pull down new xtb and translation files:

`npm run pull_l10n`

Do a commit for all the string translation updates, `xtb` files.