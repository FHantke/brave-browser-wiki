# chromium-fork update January 17, 2018

This page is meant to give you a snapshot in time view of:

- Where we're at with development and how it's split up into milestones.
- Current staffing.
- Hiring plan.
- Give a couple screenshots of where we're at.

## Milestones


### Milestone 1: Open source (December-February)
- [x] Adblock off the main thread
- [x] Tracking protection off the main thread
- [x] HTTPS Everywhere off the main thread
- [x] Browser action for shields panel, implemented through an extension
- [x] Packaging installer by default
- [x] macOS installer
- [x] Windows silent installer
- [x] Signing on macOS
- [x] Basic JS blocking via shields
- [x] URL bar suggestions from Alexa and search suggestions off by default
- [x] Debranding
- [x] WebUI basic framework used by payments, new tab page and welcome page
- [x] geolocations
- [x] produce build on Linux / Win that changes tab shape
- [ ] Signing build / installer on Windows
- [ ] Linux debs / rpms w/ signing.
- [ ] No phoning home to Google (mostly done but more work remains)
- [ ] Removing Chrome extension store support
- [ ] Payments / Ledger impl (In progress)
- [ ] new tab page impl
- [ ] welcome page impl
- [ ] produce build on macOS that changes tab shape

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
  - Completed: Basic Windows and macOS packaging.
  - Completed: WebUI framework.
  - Working on Chromium rebases as they happen to keep us up to date.
  - Currently working more on landing WebUI and packaging PRs.
- Alexey Barabash recently started spending most of his time on chromium-fork
  - Completed: URL suggestions using Alexa and no search by default.
  - Working on tab shape, has a prototype of that on Windows / Linux already.
- Brian Johnsons is advising and doing code reviews. May jump in for active dev if muon slows down, did some strings / branding work initially.
- Kevin Lawler has helped with patch splitting, but is currently source for a different project.
- Sergey Zhukovsky is not working on chromium-fork directly but the native ledger-client which we'll use.
- Sriram Venkataram is lightly testing builds as they become available.
- James has helped with icon image resources.

## Hiring

- We're looking at expanding the team by 2 people in the short term to help deliver chromium-fork.
- Jocelyn will be joining the security team in Feb 12 to help.
- eV is helping to find a devops / dev candidate that can help get buildbot setup and work on the update server.


## Screenshots from chromium-fork

![screen shot 2018-01-17 at 12 01 59 pm](https://user-images.githubusercontent.com/831718/35055946-7713010c-fb7e-11e7-821d-48b17453bb69.png)
![screen shot 2018-01-17 at 12 02 18 pm](https://user-images.githubusercontent.com/831718/35055947-772568ba-fb7e-11e7-9568-eb58e72e2ede.png)

