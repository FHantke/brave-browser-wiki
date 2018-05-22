# Status Update

# Recent news

# Past communicated updates

- [February 26, 2018](https://github.com/brave/brave/wiki/Chromium-fork-Update/fd077a7e7aaaa734c31eea2290c7e2d31567f6ca)
- [February 12, 2018](https://github.com/brave/brave/wiki/Chromium-fork-Update/f5823e63e22e4c3420f86693acecc5afa1a14f67)
- [February 3, 2018](https://github.com/brave/brave/wiki/Chromium-fork-Update/e93dd33543cfc9d18e743ebd9ec55b99971c3fc4)
- [January 17, 2018](https://github.com/brave/brave/wiki/Chromium-fork-Update/d041d44b5d6213be19bf6fb951a21aa3c2c8bb77)

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
- [ ] CI for builds using buildbot (mattb in-progress)
- [ ] Update client and server work to do updates (jarv, simon in-progress)
- [x] Multiple channels support
- [x] Fine tune Chrome default settings
- [ ] Extend UI to show installable extensions in about:extensions
- [ ] Fix any remaining chromium branding things [here](https://github.com/brave/brave-browser/issues/212).
- [x] Automated browser tests framework
- [ ] Get automated tests running on Travis
- [ ] Verify no calls are made out to Google
- [x] Site hacks
- [x] New tab private search engine selection UI
- [ ] Signing on macOS (bbondy in-progress)

### Milestone 3: Getting to 1.0 (June-July)

- [ ] Import from browser-laptop based Brave (Garrett in-progress)
- [ ] More permissions
- [ ] Restyling bookmarks (Cezar)
- [ ] Restyling history
- [ ] Restyling preferences
- [ ] Extensions installation page
- [ ] Sync (Alexey in-progress)
- [ ] Flash
- [ ] Widevine
- [ ] Some UI customizations
- [ ] New private tab search engine functionality
- [ ] Linux deb and rpm signing.
- [ ] Shortcut lists
- [ ] Payments / Ledger 2.0 impl (Nejc)
- [ ] Get proper Debug builds working

### Milestone 4: Stretch goals but wanted for 1.0

- [ ] Session tabs (Brian Johnson)
- [ ] Tor private tabs (Taylor Campbell)
- [ ] Webtorrent
- [ ] Refine tab shape to be better (Waiting on design from Brad's team)
- [ ] Autoplay (Anthony in-progress)
- [ ] Clear private data on shutdown

### Unlikely stretch goals

- [ ] Private tabs
- [ ] More UI customizations
- [ ] Tab pages
- [ ] Stub installer wrapper with UI for Windows using Mozilla stub installer. (Emerick will start on this after extension work)

## Screenshots from chromium-fork

<img width="1208" alt="screen shot 2018-02-26 at 9 39 46 am" src="https://user-images.githubusercontent.com/831718/36676797-9c6d26aa-1ada-11e8-95b9-c5a7bfb0cd8e.png">

<img width="1034" alt="screen shot 2018-02-03 at 2 25 56 pm" src="https://user-images.githubusercontent.com/831718/35770707-3f4b819a-08ee-11e8-88e0-69d9be4abae0.png">