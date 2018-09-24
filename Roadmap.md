## Milestones

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
- [x] Jumbo UI on Windows (Pete)
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
- [x] URL bar centered changes (Pete)
- [ ] Fix Widevine
- [ ] Default Brave shield preferences
- [ ] Manage script should not show enforced by extension
- [ ] Referral promo (Emerick)
- [ ] Uphold widget (Nejc)
- [ ] Sync (Alexey, Cezar)
- [ ] 2-way wallet? (Nejc)
- [ ] Get automated C++ tests running on Travis

### Milestone 6: Releasable builds for 0.56.x -- candidate to rename to 1.0 (November)

- [ ] Ads
- [ ] More UI work
- [ ] Tipping and donations
- [ ] Make extensions.brave.com store
- [ ] Restyling bookmarks, history, preferences (Cezar)

### Beyond release Milestone 6

- [ ] Private tabs instead of just private windows
- [ ] More UI customizations
- [ ] Tab pages?
- [ ] Tab previews
- [ ] Translations (Chrome parity)
- [ ] Pinned tab differences
- [ ] Clear private data on shutdown
- [ ] Session windows (Brian Johnson)
