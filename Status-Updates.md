# Status Update

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
- [x] UGP (Nejc)
- [x] Allow a different private search engine to tie into private newtab UI
- [ ] Cosmetic filter blocking (Snuupy)
- [ ] Manage script should not show enforced by extension
- [ ] Tor private windows (Taylor Campbell, Anthony, Emerick)
- [ ] Webtorrent (Jocelyn)
- [ ] Referral promo (Emerick)
- [ ] Bookmarks star button on the left (Pete)
- [ ] Colors and tab shape (Pete)
- [ ] Extensions installable from CWS, updatable (bbondy)
- [ ] Dev channel dark theme (bbondy)
- [ ] Chromium 70 rebase (bbondy)
- [ ] Moving BAT & Brave icon into URL bar (Pete)

### Milestone 5: Beta channel Releasable builds for 0.55.x -- released on brave.com replacing browser-laptop (September 2018)

- [ ] Uphold widget (Nejc)
- [ ] Verify no calls are made out to Google (pj)
- [ ] Make extensions.brave.com store (Ailin)
- [ ] Hide settings which are not applicable, add Brave specific needed settings. (Pete or Cezar)
- [ ] Restyling bookmarks, history, preferences (Cezar)
- [ ] Sync (Alexey, Cezar)
- [ ] 2-way wallet? (Nejc)
- [ ] More permissions (Pete)
- [ ] Tab bar changes (Pete)
- [ ] URL bar centered changes (Pete)
- [ ] Get automated C++ tests running on Travis

### Milestone 6: Beta channel Releasable builds for 0.56.x -- candidate to rename to 1.0 (October 2018 - November 2018)

- [ ] Ads
- [ ] More UI work

### Beyond Milestone 6

- [ ] Private tabs
- [ ] More UI customizations
- [ ] Tab pages
- [ ] Tab previews
- [ ] Translations (Chrome parity)
- [ ] Pinned tab differences
- [ ] Clear private data on shutdown
- [ ] Session windows (Brian Johnson)
