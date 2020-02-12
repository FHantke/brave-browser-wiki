See the [Roadmap page](https://github.com/brave/brave-browser/wiki/Roadmap) for recent version information.

## Milestones

This is a working document, things will update and move around to reflect reality. 
Things are not set in stone in this document.  Brave members are expected to update this document to reflect reality to share current status with community.

Older milestones are available in the [Roadmap Archive page](https://github.com/brave/brave-browser/wiki/Roadmap-Archive).

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

### Milestone 0.64.x (~May 14, 2019) - Completed & Released

- [x] Custom ABP filter rules in about:adblock (Emerick, in progress)
- [x] UI improvements and support for theming in the Brave shields panel
- [x] Support for Nightly builds
- [x] Override regional ad-block selection in about:adblock

### Milestone 0.65.x (~June 4, 2019) - Completed & Released
- [x] Smart Tracking Protection (off by default)

### Milestone 0.66.x - (~July 2, 2019) Completed & Released

- [x] In page translations (Jocelyn, completed in 0.65.x but not enabled on Release channel for cost performance needs)

### Milestone 0.67.x - (~August 1, 2019) Completed & Released

- [x] Ability to compile Brave and produce an Android based APK (nothing close to releasable yet)
- [x] Tie Brave Core Android builds to CI
- [x] Twitter tipping
- [x] Build a Rust based ad-block library with best of class performance.


### Milestone 0.68.x - (~August 21, 2019) Completed & Released

- [x] Rust based ad-block integration
- [x] Ongoing work for Brave Core Android builds

### Milestone 0.69.x - (~October 3, 2019) Completed & Released

- [x] Ethereum Wallet - [#4494](https://github.com/brave/brave-browser/issues/4494)
- [x] User Wallets/Uphold Connect - [#4774](https://github.com/brave/brave-browser/issues/4774)
- [x] Added onboarding for User Wallets/Uphold Connect - [#5518](https://github.com/brave/brave-browser/issues/5518)
- [x] GitHub panel and inline tipping - [#4987](https://github.com/brave/brave-browser/issues/4987) & [#5040](https://github.com/brave/brave-browser/issues/5040)
- [x] User Ads Feedback / Ad Policy Operations - [#4047](https://github.com/brave/brave-browser/issues/4047)
- [x] Chromecast support - [#209](https://github.com/brave/brave-browser/issues/209)
- [x] Ability to change default theme via Welcome onboarding experience - [#2909](https://github.com/brave/brave-core/pull/2909)
- [x] Ability to import bookmarks via the Welcome onboarding experience - [#1530](https://github.com/brave/brave-browser/issues/1530)
- [x] Ability to hide widgets on New Tab Page (Brave Stats, Clock, Top Sites) - [#4510](https://github.com/brave/brave-browser/issues/4510)
- [x] Add “Simple” view for Brave Shields - [#1196](https://github.com/brave/brave-browser/issues/1196)
- [x] Ability to use “Simple” or “Advanced” Brave Shield view as global default using brave://settings - [#4784](https://github.com/brave/brave-browser/issues/4784)
- [x] Automatic DApp detection - [#718](https://github.com/brave/brave-browser/issues/718)
- [x] Making New Tab Page keyboard accessible - [#5041](https://github.com/brave/brave-browser/issues/5041)
- [x] Several WebTorrent fixes and general improvements
- [x] Ability to turn off Widevine modals via “Don’t ask again” checkbox - [#2980](https://github.com/brave/brave-core/pull/2980)
- [x] Translations via Google Translate extension - [#5561](https://github.com/brave/brave-browser/issues/5561)
- [x] Bookmark bar visible under New Tap Page by default - [#5781](https://github.com/brave/brave-browser/issues/5781)
- [x] Ability to change “Always show bookmark bar” on New Tab Page via brave://settings - [#4782](https://github.com/brave/brave-browser/issues/4782)
- [x] Optimized publishers list database - [#5907](https://github.com/brave/brave-browser/issues/5907)

### Milestone 0.70.x - (~October 24, 2019) Completed & Released

- [x] Ability to disable the Brave Shields activity count using brave://settings - [#3121](https://github.com/brave/brave-browser/issues/3121)
- [x] Add Japan as a supported region for Brave ads - [#5656](https://github.com/brave/brave-browser/issues/5656)
- [x] Ability to disable Brave Wallet using brave://settings  - [#5673](https://github.com/brave/brave-browser/issues/5673)
- [x] Ability to save all files from WebTorrent via “Save All Files” - [#1230](https://github.com/brave/brave-browser/issues/1230)
- [x] Greaselion - [#5674](https://github.com/brave/brave-browser/issues/5674)
- [x] Simplifying Profile Manager
- [x] Improving Brave ads frequency so same ad isn’t displayed more than once per hour - [#5281](https://github.com/brave/brave-browser/issues/5281)

### Milestone 0.73.x

- [ ] Desktop Bookmark Platform Alignment (Will improving Syncing across platforms - [#5158](https://github.com/brave/brave-browser/issues/5158)
- [ ] Terminating/Disabling Rewards extension process when disabled Rewards - [#3436](https://github.com/brave/brave-browser/issues/3436)
- [ ] WebTorrent extension should be non-persistent - [#6372](https://github.com/brave/brave-browser/issues/6372)

### Milestone 0.72.x

- [ ] New monthly contribution banner for publishers - [#5996](https://github.com/brave/brave-browser/issues/5996)
- [ ] Ability to disable Tor via admin policy - [#454](https://github.com/brave/brave-browser/issues/454)
- [ ] WebCompat Reporter via (Report a broken site)

### Beyond

- [ ] Ad-block exceptions to be disabled by default and have UI asking to turn on when it is first detected (todo design)
- [ ] Add the ability to have social media login options into the Brave shields panel
- [ ] Add support for more locales
- [ ] Sync v2
- [ ] Speedreader
- [ ] Dark mode for content
- [ ] Playlist
- [ ] BYOW Rewards
- [ ] Make extensions.brave.com store
- [ ] Private tabs instead of just private windows
- [ ] More UI customizations to differentiate from Chrome look
- [ ] Tab pages?
- [ ] Tab previews
- [ ] Pinned tab differences
- [ ] Session windows
- [ ] Restyling bookmarks, history, preferences
- [ ] IPFS integration
- [ ] Uphold widget / 2 way wallet?
- [ ] Tipping on individual sub-content
- [ ] Sync for other types of data