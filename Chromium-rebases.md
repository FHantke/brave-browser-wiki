## Configure and init the new Chromium version.

Create a new cloned dir.

`npm install`

Open `package.json` in the root repo and change the string at the path `config.projects.chrome.dir.tag` to the new Chromium version.

Since you’re using a new cloned dir you should not already have an `src` dir. 

`npm run init`

If patches apply correctly, simply run `npm run update_patches` and commit the changes.

## Fix patches.

If patches do not apply, then stash any changes:

```
cd src/
git stash
```

Get a list of the patches:
`ls -la brave/patches/ | mvim -`

Do a vim git macro to add `git apply -3 ./brave/patches/` before each line.
Save the list of `git apply` script file `:wq a`

(Alternatively: `ls -1 brave/patches/*.patch | sed 's!^!git apply -3 ./!' > a`)

Make it runnable:

`chmod u+x ./a`

Run it:

`./a`

You should see a bunch of files that were a) applied successfully, b) failed to apply but worked on 3-way merge, c) failed to apply and failed to apply on 3-way merge, and d) could not be applied because the file no longer exists (moved most likely).

Copy out the files that did not apply, you should have a list something like this:

```
error: patch failed: chrome/app/chrome_main.cc:7
error: patch failed: chrome/browser/browser_process_impl.cc:987
error: patch failed: chrome/browser/chrome_browser_main.cc:978
error: patch failed: chrome/browser/content_settings/host_content_settings_map_factory.cc:81
error: patch failed: chrome/browser/extensions/component_extensions_whitelist/whitelist.cc:9
error: chrome/browser/ui/cocoa/new_tab_button.mm: No such file or directory
error: patch failed: chrome/browser/ui/cocoa/tabs/tab_controller.mm:101
error: patch failed: chrome/browser/ui/layout_constants.cc:25
error: patch failed: chrome/browser/ui/webui/chrome_web_ui_controller_factory.cc:11
error: patch failed: chrome/common/BUILD.gn:528
error: patch failed: chrome/common/importer/BUILD.gn:12
error: patch failed: chrome/common/importer/profile_import.mojom:7
error: patch failed: chrome/installer/mini_installer/BUILD.gn:129
error: patch failed: chrome/renderer/chrome_content_renderer_client.cc:20
error: patch failed: chrome/utility/chrome_content_utility_client.h:36
error: patch failed: components/vector_icons/vector_icons.gni:40
error: patch failed: extensions/browser/extension_event_histogram_value.h:430
error: patch failed: third_party/WebKit/Source/core/svg/SVGPathElement.cpp:20
error: patch failed: third_party/WebKit/Source/modules/webgl/WebGLRenderingContextBase.cpp:2688
error: patch failed: tools/gritsettings/resource_ids:149
```

Treat this new text document as your todo list.
Open these files one by one and resolve the merge conflicts manually.

Be careful when applying `tools/gritsettings/resource_ids`, sometimes the IDs can change and you should update the Brave one to be the same as the Chromium one.

Once you are done updating all the Chromium src files, the -3 option above will actually stage files in src to be added, we don't want that because our patch generation script doesn't work well with it.  So inside the src directory run `git reset HEAD *`.  Then run `npm run update_patches`

Commit what you have in a commit to update patches for the Chromium update.

## Check for changes in the build tool chain.
* For Mac check `mac_sdk_min` in `src/build/config/mac/mac_sdk_overrides.gni` and `MAC_TOOLCHAIN_VERSION` in `src/build/mac_toolchain.py`,
* For Windows check `MSVS_VERSIONS` in `src/build/vs_toolchain.py` (`CURRENT_DEFAULT_TOOLCHAIN_VERSION` in `src/build/win_toolchain.py` prior to C75).

Update your tool chain if needed and alert dev and CI teams on tool chain changes. 

## Fix build.

Do a build like normal, fix errors as they come up.

`npm run build`

###  Fix strings.

Eventually you’ll start getting some string errors when you build.
Commit what you have in a updates to brave-core commit.

Reset Chromium strings to the original state:
```
cd src
git checkout -- *.xtb
git checkout -- *.grd
git checkout -- *.grdp
```

Update `grd` and `grdp` files from the Chromium ones:

```
cd ..
npm run chromium_rebase_l10n
```
```
Note: Filled some custom strings to brave_strings.grd by https://github.com/brave/brave-core/pull/214/commits/256f5aa8e781d266ad23f863ac7b613615ad2a5a
```

Do a commit for the updated source strings, `grd` files.

### Update localization.

Run this to detect new strings and push them to Transifex, it will also push up the translations for those strings automatically.
If you need access talk to devops.

```
npm run push_l10n
```

Run this to pull down new xtb and translation files:

`npm run pull_l10n`

Do a commit for all the string translation updates, `xtb` files.

### Fix remaining build errors.

Continue the build after that runs successfully.

If you have more fixes you can add them to the commit before the string commit which has the brave-core updates.

`git rebase -i HEAD~2` to do that.

Continue to build with:

`npm run build`

If there's anything that should be called out as a non-trivial change, you should do it as a separate commit.

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


# Notify Security team of feature changes

Check with the Security team in regards to any possibly relevant changes in features or their default settings. Specifically, post link to changes to https://chromium.googlesource.com/chromium/src/+diff/<Previous Version>..<New Version>/content/public/common/content_features.cc
in #security-discussion Slack channel. 