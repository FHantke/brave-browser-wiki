See the [Roadmap page](https://github.com/brave/brave-browser/wiki/Roadmap) for recent version information.

### Milestone 1: Open sourceable (December 2017 - February 2018)
- [x] Adblock off the main thread
- [x] Tracking protection off the main thread
- [x] HTTPS Everywhere off the main thread
- [x] Browser action for shields panel, implemented through an extension
- [x] macOS installer
- [x] Windows silent installer
- [x] Basic JS blocking via shields
- [x] URL bar suggestions from Alexa and search suggestions off by default
- [x] Debranding
- [x] WebUI basic framework used by payments, new tab page and welcome page
- [x] Geolocations
- [x] produce build on Linux / Win that changes tab shape
- [x] Signing build / installer on Windows
- [x] Linux debs / rpms
- [x] Upload symbols and bins to GitHub
- [x] Ability to override cc files for less patching to chromium src
- [x] new tab page impl
- [x] welcome page impl
- [x] produce build on macOS that changes tab shape
- [x] Restore last session by default
- [x] Disable search autocomplete and use alexa top 500
- [x] Transifex tooling and app localization
- [x] Extension localization tooling
- [x] Canvas / WebGL Fingerprinting protection

### Milestone 2: Builds with updates (March 2018 - May 2018)

- [x] Audio / WebRTC fingerprinting protection
- [x] Third party fingerprinting protection
- [x] Noscript with per origin selections
- [x] Updating DAT files via an extension.
- [x] Removing Chrome extension store support.
- [x] Create whitelist for extension installation.
- [x] Import from Chrome
- [x] Multiple channels support
- [x] Fine tune Chrome default settings
- [x] Automated browser tests framework
- [x] Site hacks
- [x] New tab private search engine selection UI
- [x] Signing on macOS
- [x] Proxy Safe Browsing
- [x] Disable Flash by default per [v1 Flash plan](https://github.com/brave/brave-browser/issues/30)
- [x] Disable PDFium and use PDF.js
- [x] Generate brave-extensions for DAT files and themes
- [x] Get proper Debug builds working
- [x] Newtab private search engine UI

### Milestone 3: Unadvertised Dev channel builds (June 2018 - July 2018)

- [x] Import from browser-laptop based Brave (Garrett in-progress)
- [x] Widevine
- [x] Jumbo UI on Windows
- [x] Linux deb and rpm signing.
- [x] Native-Ledger integration into brave-core
- [x] Rewards hooking up into delegates and observers
- [x] Stats when you click on a number in brave shields panel (Cezar)
- [x] update ping stats
- [x] Crash reporting
- [x] Local welcome page w/ placeholder
- [x] Local welcome page UI impl
- [x] Chromium 68 rebase
- [x] Fix any remaining chromium branding things [here](https://github.com/brave/brave-browser/issues/212).  (Jocelyn, in progress)
- [x] CI for builds using buildbot
- [x] Update client and server work to do updates
- [x] Windows stub installer from Omaha server
- [x] Regional ad-block
- [x] brave:// protocol support
- [x] Get automated WebUI tests running on Travis

### Milestone 4: Dev channel Releasable builds for 0.55.x (August 2018)

- [x] Chromium 69 rebase
- [x] Autoplay
- [x] Rewards UI integration into brave-core (Nejc)
- [x] UGP
- [x] Allow a different private search engine to tie into private newtab UI
- [x] Cosmetic filter blocking
- [x] Dev channel dark theme
- [x] Bookmarks star button on the left
- [x] Chromium 70 rebase
- [x] Verify no calls are made out to Google
- [x] Extensions installable from CWS, updatable
- [x] Moving BAT & Brave icon into URL bar
- [x] Colors and tab shape v1
- [x] Webtorrent
- [x] go-update component update server


### Milestone 5: Release channel Releasable builds for 0.55.x -- released on brave.com (September-October 2018)

- [x] Tor private windows
- [x] Tab shape
- [x] Hide settings which are not applicable
- [x] Colors v2
- [x] URL bar centered changes
- [x] Fix Widevine
- [x] Default Brave shield preferences
- [x] Manage script should not show enforced by extension
- [x] Referral promo
- [x] Tipping and donations

### Milestone 6: 0.56.x - Completed & Released

- [x] New shields redesign
- [x] More UI work
- [x] Get automated C++ tests running on Travis

### Milestone 6: 0.57.x - Completed & Released

- [x] Add import of more Brave legacy things
- [x] Start migrate muon users
- [x] Fix extensions compat
- [x] Fix some blocking issues

### Milestone 6: 0.58.x - Completed & Released

- [x] Tipping banner
- [x] More import settings from Brave old (search settings)
- [x] Fix relaunch after update
- [x] Fixed PDFs not loading on certain websites when they are behind basic authentication

### Milestone 6: 0.59.x - Completed & Released

- [x] Bookmarks Sync
- [x] Ads integration landing into brave-core possibly disabled behind a flag
- [x] Add Widevine detection to show up for install any time Widevine is attempted to be accessed.
- [x] Allow disabling PDF.js in settings
- [x] Upgrade to Chromium 72

### Milestone 0.60.x (~February 19, 2019) - Completed & Released

- [x] Rust support for brave-core.
- [x] Webcompat fixes (including sportsnet)
- [x] Ads integration into Brave-core with foundations to support Rust based blind token confirmations.  This will be in a disabled state in Release channel until related work is completed in future milestones.
- [x] Build out PR builder to do builds for each PR
- [x] Build out daily Dev channel and Beta builds CI
- [x] Fix Twitch tipping
- [x] New tab UI tweaks
- [x] Rewards stability fixes

### Milestone 0.61.x (~March 12, 2019) - Completed & Released

- [x] More ads work. This will be in a disabled state in Release channel until related work is completed in a future milestones.
- [x] Upgrade to Chromium 73
- [x] Rewards stability fixes
- [x] Webcompat fixes including third-party detection and ad-block rule parsing fixes

### Milestone 0.62.x (~April 2, 2019) - Completed & Released

- [x] Widevine support for Linux
- [x] More ads work
- [x] Add options for allowing FB login / embeds and Twitter embeds
- [x] MacOS OS theme controls theme

### Milestone 0.63.x (~April 23, 2019) - Completed & Released

- [x] Unify exception handling 
- [x] Fix Brave shields localization
- [x] Make Widevine UI more noticeable
- [x] eTLD+1 matching for about:adblock
- [x] Working ads model, Rust-based blinded tokens for privacy-safe confirmation of ad views and interaction, and ability to give BAT monthly to users who see ads
- [x] Settings/Bookmarks/History/Downloads facelift
- [x] Clear private data on exit
- [x] Upgrade to Chromium 74
