# Background #
* What is CDM?
  * A Content Decryption Module (CDM) refers to the client-side DRM component of an application which performs the decryption, decoding, or enables playback of encrypted video content. Different platforms use different CDM technology. ([source](https://castlabs.com/resources/faq/drm/))

* What is Widevine?
  * Widevine is a digital rights management component of the Google Chrome web browser and Android MediaDRM originally created by Widevine Technologies
  * Resources
    * [wiki](https://en.wikipedia.org/wiki/Widevine)
    * [official page](https://www.widevine.com)

* Terms in this document
  * `cdm` refers widevine cdm
  * `cdm lib` refers widevine cdm library
  * `cdm service` is the utility process that provides service to the media pipeline running in the renderer process. It loads and runs cdm lib.
  * `CDM` refers general Content Decryption Module

* How CDM works in chrome?
  * Chrome ships cdm lib by default. On MacOS and Windows, cdm lib is installed/updated by component updater. However, chrome on linux delivers cdm lib within the distribution package. So, its update is only done by chrome update. I assume this is because of the use of zygote process in chrome linux. Chrome launches cdm service when renderer wants and that service process is forked from running zygote process. At that time, cdm service can't use cdm library if zygote process doesn't have widevine library. When zygote gets fork request, it is already in the sandboxed state. In the sandboxed state, any chromium process can't load library from filesystem. On chrome linux, cdmlib is loaded during the zygote process initialization phase and it is initialized when cdm service is launched. Then it provides cmd service to renderer.
  * During the browser process initialization, available cdm lib lists fetched by content client api - (`ChromeContentClient::AddContentDecryptionModules()`) and registered to `content::CdmRegistry`. In the browser process, we can get available CDM info by using it. `KeySystemSupport` service is wrapper of `CdmRegistry`.
  * During zygote process startup, `PreloadLibraryCdms()` dynamically loads cdm lib with cdm infos(file path). Available CDM info is also provided by content client api. This timing is before entering the sandboxed state. So, cdm lib can be loaded into process.
  * We can also get cdm lib path by `base::PathService::Get(chrome::FILE_WIDEVINE_CDM, cdm_path)`. Of course, `CdmRegistry` also has path info. Brave overrides this file path.(you can check about it more)
  * When renderer needs CDM service, cdm service process is launched by forking zygote process and `CdmModule` initializes cdm lib. Then CDM service starts its role.

# How Brave enables Widevine? #
## brave-core changes ##
* Main idea
  * Reuse most of chromium's widevine bundling infra except default widevine shipping to minimize modifying upsteam files and introducing many patch files

* Enabling steps
  1. Enable two widevine gn variables  (`widevine.gni`)
     * enable_widevine
     * bundle_widevine_cdm
     * With enabling two gn variables, Brave browser process and zygote process do same jobs as specified above.(`CdmRegistry` and `PreloadLibraryCdms()`). However, CdmRegistry is empty and cdm lib isn't loaded because there is no cdm lib in filesystem that `chrome::FILE_WIDEVINE_CDM` points. When renderer wants to use CDM service, CDM service is launched but its initializing is failed because no cdm lib is loaded.

  2. Enable content settings bubble
     * Brave only downloads and uses cdm lib when user accepts. When blocked image is visible in omnibox, user can know this url needs cdm lib.
     * They are `BraveWidevineBlockedImageModel` and `BraveWidevineContentSettingPluginBubbleModel` and only enabled with `ENABLE_WIDEVINE_CDM_COMPONENT`.
     * So, enable them also with `BUNDLE_WIDEVINE_CDM` build flag.

  3. Download and update cdm lib
     * When user accpets using cdm lib, `BraveWidevineBundleManager` downloads, unzips(`BraveWidevineBundleManager` and store it to `chrome::FILE_WIDEVINE_CDM`. That path is changed to user data dir because Brave downloads and update during the runtime. When `BraveWidevineBundleManager` finishes its role, blocked image and content settigns bubble guide users to restart. Installed cdm lib is only loaded at the startup of zygote, Brave needs to restart.

## brave-browser changes ##
* To build Brave with bundling mode, there are some prerequisites. They are `widevine_cdm_verison.h`(header file that specifies version of bundled cdm lib), `libwidevinecdm.so`(cdm lib), and `manifest.json`. Version string and cdm lib is used in core when creating `CdmInfo` and zygotes loads. So, gn file checks them the existence of during the build. So, brave-browser prepares version file and cdm lib. This cdm lib is fake one. Fake cdm lib is just for build time. During the runtime, Brave uses real cdm lib that we downloads. To do that, `chrome::FILE_WIDEVINE_CDM` is changed. `manifest.json` is left empty (`{}`) as we are not using CDM for bundling.

## Widevine version update ##
When we need to update widevine, we just need to set new version string to `config.widevine.version` in `brave-browser/package.json`. Latest version can be checked by url that google provides. That documents said Widevine supports current version and two prior versions. So, version change would be sufficient when we do major version update.

## Issue and PRs ##
* https://github.com/brave/brave-browser/issues/413
* https://github.com/brave/brave-browser/pull/3300
* https://github.com/brave/brave-core/pull/1606
* https://github.com/brave/brave-core/pull/1668

* Default shipping approach(not merged)
  * This approach is not accepted because of cdm licence issue
  * https://github.com/brave/brave-browser/pull/3037
  * https://github.com/brave/brave-core/pull/1475