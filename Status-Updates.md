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
- [x] Signing on macOS
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
- [ ] CI for builds using buildbot
- [ ] Update client and server work to do updates
- [x] Multiple channels support
- [ ] Fine tune Chrome default settings
- [ ] Extend UI to show extensions in about:extensions
- [ ] Fix any remaining chromium branding things.
- [x] Automated browser tests framework
- [ ] Get automated tests running on Travis
- [ ] No phoning home to Google (mostly done but more work remains)
- [x] Site hacks

### Milestone 3: Getting to 1.0 (June-July)

- [ ] Import from browser-laptop based Brave
- [ ] More permissions
- [ ] Restyling bookmarks
- [ ] Restyling history
- [ ] Restyling preferences
- [ ] Extensions installation page
- [ ] Sync
- [ ] Flash
- [ ] Widevine
- [ ] Some UI customizations
- [ ] New tab private search engine selection (DuckDuckGo)
- [ ] Linux deb and rpm signing.
- [ ] Shortcut lists
- [ ] Payments / Ledger impl
- [ ] Get proper Debug builds working

### Milestone 4: Stretch goals but wanted for 1.0

- [ ] Session tabs
- [ ] Tor private tabs (Taylor Campbell)
- [ ] Webtorrent
- [ ] Refine tab shape to be better (Waiting on design from Brad's team)
- [ ] Autoplay
- [ ] Clear private data on shutdown

### Unlikely stretch goals

- [ ] Private tabs
- [ ] More UI customizations
- [ ] Tab pages
- [ ] Stub installer wrapper with UI for Windows using Mozilla stub installer. (Emerick will start on this after extension work)


## Staffing

- Full time resources:
  - Brian Bondy
  - Alexey Barabash
  - Jocelyn Liu
  - Emerick Rogul
  - Garrett Robinson

- Pasts part time resources:
  - Cezar Augusto worked on the shields panel UI but is not currently sourced on the project.
  - Nejc Zdovc has worked on porting the brave-extension for the shields panel to typescript.
  - Brian Johnsons is advising and doing code reviews. May jump in for active dev if muon slows down, did some strings / branding work initially.
  - Kevin Lawler has helped with patch splitting, but is currently sourced for a different project.
  - Sergey Zhukovsky is not working on chromium-fork directly but the native ledger-client which we'll use.
  - Sriram Venkataram is lightly testing builds as they become available.
  - James Mudgett has helped with icon image resources.
  - Anthony Tseng has helped with some build problems on Windows and did some work towards debranding but is not currently sourced on this project.  He may help out more if opportunities come up between muon updates.

## Hiring

- There's no active hiring being done, we're fully staffed up for Chromium-fork!

## Screenshots from chromium-fork

<img width="1208" alt="screen shot 2018-02-26 at 9 39 46 am" src="https://user-images.githubusercontent.com/831718/36676797-9c6d26aa-1ada-11e8-95b9-c5a7bfb0cd8e.png">

<img width="1034" alt="screen shot 2018-02-03 at 2 25 56 pm" src="https://user-images.githubusercontent.com/831718/35770707-3f4b819a-08ee-11e8-88e0-69d9be4abae0.png">