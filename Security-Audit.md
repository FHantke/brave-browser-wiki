Component updater:
- brave component updater has its own configurator 
 `brave/browser/component_updater/brave_component_updater_configurator.cc`. Take special note of all values here, in particular if `EnabledCupSigning` is ok to keep disabled. (We were ok to have this disabled in muon previously)

Flash:
- Flash has no UI if it is not installed.  Flash is disabled by default via an explicit generic wildcard content setting to disable. Once you allow once, it will set a content setting to enable click to play for that origin.

Content settings:
- Content settings for all shields are done through the plugins content setting type with different resource identifiers.

Extension handling:
- Some extension IDs are whitelisted for tests, those private keys are in the clear in our repo.
- Brave shields icon is a browser action loaded from an extension at brave/brave-extension, it is included as part of a pak file.
- Extensions are restricted, by PWAs are allowed.
- DAT files updated through extensions which are the source of adblock list updates, tracking protection list updates, and HTTPS Everywhere list updates.
- Extension permission should be reviewed here: `common/extensions/api/_api_features.json`

Allowed extensions
- There is a whitelist of extensions allowed to be installed otherwise at: `brave/extensions/browser/brave_extension_provider.cc`
- The plan is to allow 1k+ extensions initially.

Added Extension APIs:
- Some extra extension APIs are available for blocking information here: `common/extensions/api/brave_shields.json`.  It currently applies to all `blessed_extension`s.

Tooling:
- Translations get pulled down from Transifex and new source strings get pushed there on demand and during chromium upgrades.

Enabled/disabled features over Chromium:
- Enabled progressive web apps flag (PWAs) (https://github.com/brave/brave-core/commit/3c046d3a419e4585d5c6f7b1f6d5963ce8a5902b)
- Emoji picker control when right clicking in an input text control.
- Tab muting is enabled by default (icon clickable in tabs)
- Sign into Chrome/Brave feature disabled `brave/browser/brave_profile_prefs.cc`
- Restoring last tab value default pref value set to remember last session `brave/browser/brave_profile_prefs.cc`

Tests:
- Tests: We are currently running our own browser tests and unit tests, we are not currently running Chromium unit and browser tests, but we'd like to enable some subset of them that apply.

PDF handling:
- PDF handling via the Chromium PDF Viewer extension is explicitly disabled by blocking the extension ID.
- PDF.js is force installed on startup via a setting that's used for administration management policy. It can't currently be disabled, but if we decide to allow that we can make it part of the recommended extension list instead.
- There is a manifest override handler which only applies to PDF.js and nulls out the page action here `brave/common/extensions/manifest_handlers/pdfjs_manifest_override.cc`.

Keychain:
- I believe we store our passwords in our own keychain, I believe we will ask to unlock the keychain of other apps during imports.  (Check with Garrett and/or Anthony)

HTTPS Redirects:
- HTTPS redirects are driven in the `brave/browser/net/brave_httpse_network_delegate_helper.cc` file which loads a leveldb loaded from an extension.

Tracking protection, Ad-block:
- Managed by code in `brave/components/brave_shields/browser/tracking_protection_service.cc` and `brave/components/brave_shields/browser/ad_block_service.cc`
- Works from lists updated in an extension that holds only a DAT file. The DAT file is loaded as binary in the same way as muon handles them by the same library.

Site hacks:
- Site hacks should be reviewed individually, they are managed here: `brave/browser/net/brave_site_hacks_network_delegate_helper.cc`. They run only for the profile level network delegate.
- Shield exceptions are managed here `brave/common/shield_exceptions.cc`

Browser level network delegate helper:
- Network redirects mapping Chrome URLs to Brave ones are done for the browser context here: `brave/browser/net/brave_static_redirect_network_delegate_helper.cc`.

Software updates:
- Sparkle (macOS)
- Omaha (Windows)
- Check with Jarv and Simon Hong for details.

Custom `about:` pages:
- Newtab page (it currently does outgoing network requests to s3)
- Welcome page (currently remote, but changing to local before we ship, muon is remote though).

Cookie blocking:
- Cookie blocking is managed here: `brave/browser/brave_content_browser_client.cc`

Source code overrides:
- Source code overrides should get special attention, in these cases we completely replace a Chromium file with one of our own.  These files exist in `src/brave/chromium_src` and if a file exists there it will overwrite the equivalent file in `src`.

Command line switches:
- Brave specific command line switches exist here: `brave/common/brave_switches.cc`.

Brave specific preferences:
- Brave specific preferences are added here: `brave/common/pref_names.cc`.  They should be reviewed to make sure we can't cause any harm with values that are set by different extensions.