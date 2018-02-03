# chromium-fork Update

# Recent changes tldr;

- Linux deb and rpms work done and almost landed
- Welcome page done and landed for first run
- New tab page landed, not showing stats count yet but shows top sites
- Tab shape on macOS (previously also on Windows / Linux)
- Restores last session now enabled by default
- Upgraded to Chromium 64
- New candidate starting February 12th to work on #chromium-fork
- New candidate hired likely to start March 1st
- Plan to use partitions for session tabs


# Key Updates:
- [January 17, 2018](https://github.com/brave/brave/wiki/Chromium-fork-Update/d041d44b5d6213be19bf6fb951a21aa3c2c8bb77)
- [February 3, 2018](https://github.com/brave/brave/wiki/Chromium-fork-Update/fa87057f8f31ceb5b90c7f670035e533b214e942)

## Milestones


### Milestone 1: Open source (December-February)
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
- [ ] Linux signing.
- [ ] No phoning home to Google (mostly done but more work remains)
- [ ] Removing Chrome extension store support
- [ ] Payments / Ledger impl
- [x] new tab page impl
- [x] welcome page impl
- [x] produce build on macOS that changes tab shape
- [x] Restore last session by default
- [x] Disable search autocomplete and use alexa top 500

### Milestone 2: Builds with updates (March-May)

- [ ] Installer wrapper with UI for Windows
- [ ] More advanced JS blocking via shields
- [ ] Update client and server work to do updates
- [ ] Refine tab shape to be better
- [ ] Multiple channels support
- [ ] Fine tune Chrome default settings
- [ ] CI for builds using buildbot
- [ ] Provide alternate UI to install extensions
- [ ] Investigate updating DAT files via an extension.
- [ ] Fix any remaining chromium branding things.

### Milestone 3: Getting to 1.0 (May-July)

- [ ] Import from Brave
- [ ] More permissions
- [ ] Restyling bookmarks
- [ ] Restyling history
- [ ] Restyling preferences
- [ ] Clear private data on shutdown
- [ ] Extensions installation page
- [ ] Sync
- [ ] Flash
- [ ] Widevine
- [ ] Some UI customizations

### Milestone 4: Stretch goals or post 1.0

- [ ] Private tabs (Brian Johnson)
- [ ] Session tabs (Brian Johnson)
- [ ] Tab pages
- [ ] More UI customizations
- [ ] Tor private tabs (Taylor Campbell)

## Staffing

- Brian Bondy is spending most of his coding time on chromium-fork
  - Completed: Native Adblock, tracking protection, HTTPS Everywhere, basic DAT file downloading
  - Completed: C63 upgrade
  - Completed: Basic Windows and macOS packaging and signing.
  - Completed: WebUI framework.
  - Completed: Geolocations
  - Completed: C64 upgrade
  - Completed: Basic newtab page
  - Completed: Welcome page
  - Completed: Restore session by default

- Alexey Barabash recently started spending most of his time on chromium-fork
  - Completed: URL suggestions using Alexa and no search by default.
  - Completed: tab shape on Windows / Linux
  - Completed: tab shape on macOS
  - Completed: Top site suggestions
  - Completed: Packaging on Linux

- Cezar Augusto worked on the shields panel UI but is not currently sourced on the project.
- Nejc Zdovc has worked on porting the brave-extension for the shields panel to typescript.
- Brian Johnsons is advising and doing code reviews. May jump in for active dev if muon slows down, did some strings / branding work initially.
- Kevin Lawler has helped with patch splitting, but is currently sourced for a different project.
- Sergey Zhukovsky is not working on chromium-fork directly but the native ledger-client which we'll use.
- Sriram Venkataram is lightly testing builds as they become available.
- James Mudgett has helped with icon image resources.
- Anthony Tseng has helped with some build problems on Windows and did some work towards debranding but is not currently sourced on this project.  He may help out more if opportunities come up between muon updates.

## Hiring



- Jocelyn will be joining the security team in Feb 12 to help.
- Offer is sent for another candidate who is likely to accept and start on March 1st. 
- Short term contractor likely to take on work soon.
- eV is helping to find a devops / dev candidate that can help get buildbot setup and work on the update server.


## Screenshots from chromium-fork

<img width="1034" alt="screen shot 2018-02-03 at 2 25 56 pm" src="https://user-images.githubusercontent.com/831718/35770707-3f4b819a-08ee-11e8-88e0-69d9be4abae0.png">