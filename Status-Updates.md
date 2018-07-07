# Status Update

## Milestones

### Milestone 1: Open sourceable (December-February)
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

### Milestone 2: Builds with updates (March-May)

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

### Milestone 3: Getting to 1.0 (June-July)

- [ ] Make extensions.brave.com store (Ailin)
- [ ] Import from browser-laptop based Brave (Garrett in-progress)
- [ ] More permissions
- [ ] Restyling bookmarks, history, preferences (Cezar)
- [ ] Sync (Alexey in-progress)
- [x] Widevine
- [ ] Some UI customizations (Pete)
- [ ] Linux deb and rpm signing.
- [ ] Payments / Ledger 2.0 impl (Nejc)
- [ ] Get automated tests running on Travis
- [ ] Hide settings which are not applicable, add Brave specific needed settings.
- [ ] Verify no calls are made out to Google (pj)
- [ ] Allow a different private search engine to tie into private newtab UI (Simon)
- [x] Stats when you click on a number in brave shields panel (Cezar)
- [ ] Manage script should not show enforced by extension
- [x] update ping stats
- [ ] Crash reporting (Matt O / Aubrey / emerick)
- [ ] Local welcome page (Cezar)
- [ ] Referral promo (& Dow Jones)
- [ ] UGP
- [ ] Chromium 68 rebase
- [ ] Fix any remaining chromium branding things [here](https://github.com/brave/brave-browser/issues/212).  (Jocelyn, in progress)
- [x] CI for builds using buildbot
- [x] Update client and server work to do updates
- [x] Windows stub installer from Omaha server
- [x] Regional ad-block
- [x] brave:// protocol support


### Milestone 4: Wanted for 1.0 (August - September first week)

- [ ] Session windows (Brian Johnson)
- [ ] Tor private windows (Taylor Campbell)
- [ ] Webtorrent (Jocelyn)
- [ ] Refine tab shape to be better (Waiting on design from Brad's team, Pete to do work for it)
- [ ] Autoplay (Anthony in-progress)
- [ ] Cosmetic filter blocking (Snuupy)
- [ ] Uphold widget
- [ ] Chromium 69 rebase
- [ ] Get update plan from muon and execute it

### Unlikely stretch goals (Can happen sometimes after September)

- [ ] 2-way wallet
- [ ] Private tabs
- [ ] More UI customizations
- [ ] Tab pages
- [ ] Tab previews
- [ ] Translations (Chrome parity)
- [ ] Pinned tab differences
- [ ] Clear private data on shutdown