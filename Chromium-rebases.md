Create a new cloned dir.

`npm install`

Open `package.json` in the root repo and change the string at the path `config.projects.chrome.dir.tag` to the new Chromium version.

Since you’re using a new cloned dir you should not already have an `src` dir. 

`npm run init`

Stash any changes if you already had a cloned dir

```
cd src/
git stash
```

Get a list of the patches:
`ls -la brave/patches/ | mvim -`

Do a vim git macro to add `git apply ./brave/patches/` before each line.
Save the list of `git apply` script file `:wq a`

Make it runnable:

`chmod u+x ./a`

You should see a bunch of errors similar to this:

```
error: patch failed: chrome/app/chrome_main.cc:7
error: chrome/app/chrome_main.cc: patch does not apply
error: patch failed: chrome/browser/browser_process_impl.cc:987
error: chrome/browser/browser_process_impl.cc: patch does not apply
error: patch failed: chrome/browser/chrome_browser_main.cc:978
error: chrome/browser/chrome_browser_main.cc: patch does not apply
error: patch failed: chrome/browser/content_settings/host_content_settings_map_factory.cc:81
error: chrome/browser/content_settings/host_content_settings_map_factory.cc: patch does not apply
error: patch failed: chrome/browser/extensions/component_extensions_whitelist/whitelist.cc:9
error: chrome/browser/extensions/component_extensions_whitelist/whitelist.cc: patch does not apply
error: chrome/browser/ui/cocoa/new_tab_button.mm: No such file or directory
error: patch failed: chrome/browser/ui/cocoa/tabs/tab_controller.mm:101
error: chrome/browser/ui/cocoa/tabs/tab_controller.mm: patch does not apply
error: patch failed: chrome/browser/ui/layout_constants.cc:25
error: chrome/browser/ui/layout_constants.cc: patch does not apply
error: patch failed: chrome/browser/ui/webui/chrome_web_ui_controller_factory.cc:11
error: chrome/browser/ui/webui/chrome_web_ui_controller_factory.cc: patch does not apply
error: patch failed: chrome/common/BUILD.gn:528
error: chrome/common/BUILD.gn: patch does not apply
error: patch failed: chrome/common/importer/BUILD.gn:12
error: chrome/common/importer/BUILD.gn: patch does not apply
error: patch failed: chrome/common/importer/profile_import.mojom:7
error: chrome/common/importer/profile_import.mojom: patch does not apply
error: patch failed: chrome/installer/mini_installer/BUILD.gn:129
error: chrome/installer/mini_installer/BUILD.gn: patch does not apply
error: patch failed: chrome/renderer/chrome_content_renderer_client.cc:20
error: chrome/renderer/chrome_content_renderer_client.cc: patch does not apply
error: patch failed: chrome/utility/chrome_content_utility_client.h:36
error: chrome/utility/chrome_content_utility_client.h: patch does not apply
error: patch failed: components/vector_icons/vector_icons.gni:40
error: components/vector_icons/vector_icons.gni: patch does not apply
error: patch failed: extensions/browser/extension_event_histogram_value.h:430
error: extensions/browser/extension_event_histogram_value.h: patch does not apply
error: patch failed: third_party/WebKit/Source/core/svg/SVGPathElement.cpp:20
error: third_party/WebKit/Source/core/svg/SVGPathElement.cpp: patch does not apply
error: patch failed: third_party/WebKit/Source/modules/webgl/WebGLRenderingContextBase.cpp:2688
error: third_party/WebKit/Source/modules/webgl/WebGLRenderingContextBase.cpp: patch does not apply
error: patch failed: tools/gritsettings/resource_ids:149
error: tools/gritsettings/resource_ids: patch does not apply
```

Copy out that string to another text file and remove the lines referencing the same files (every 2nd line usually, unless a file has moved)

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
Open these files one by one and apply the associated patch manually. 
You do this by opening the file in brave/patches with the same path but with dashes as the directory separator.
Once open you apply the changes to the associated file.

Be careful when applying `tools/gritsettings/resource_ids`, sometimes the IDs can change and you should update the Brave one to be the same as the Chromium one.

Once you are done updating all the patches remove with `rm` not `git rm` all the patches in `brave/patches` and then run `npm run update_patches`.  cd `brave/patches` and `git add` everything.

Commit what you have in a commit to update patches for the Chromium update.

Do a build like normal, fix errors as they come up.

`npm run build`

Eventually you’ll start getting some string errors when you build.
Commit what you have in a updates to brave-core commit.

Update `grd` and `grdp` files from the Chromium ones:

```
cd ..
npm run chromium_rebase_l10n
```
```
Note: Filled some custom strings to brave_strings.grd by https://github.com/brave/brave-core/pull/214/commits/256f5aa8e781d266ad23f863ac7b613615ad2a5a
```

Do a commit for the updated source strings, `grd` files.

Run this to detect new strings and push them to Transifex, it will also push up the translations for those strings automatically.
If you need access talk to devops.

```
npm run push_l10n
```

Run this to pull down new xtb and translation files:

`npm run pull_l10n`

Do a commit for all the string translation updates, `xtb` files.

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

For now just follow the instructions in https://github.com/brave/brave-browser/wiki/Tests#privacy-network-audit.

We should build some automation to find new things added here:

https://cs.chromium.org/search/?q=%22destination:+GOOGLE_OWNED_SERVICE%22&sq=package:chromium&type=cs