Feel free to copy this into an issue if you want to keep track of items per-milestone.
Be super sure that <version> is replaced with the version you wish to use.

### Prerequisites

- [ ] Double check the previous release/milestone and ensure everything has been QA'd
  - **Note:** If there's issues that need to be checked, create a `master` GH issue that lists the issues that need to be QA'd
- [ ] Consult with the security team to ensure that all security issues have been included.
   - [ ] Release using the `critical` feature (forces users to update/restart) if there's a zero day vulnerability being exploited
- [ ] Consult with PR team (@catherinecorre) and provide heads up on release timing, screenshots, other deliverables.

### Release Notes to Staging
- [ ] Mark closed issues in github as release-notes/exclude or release-notes/include.
- [ ] Commit release notes to [CHANGELOG_DESKTOP.md](https://github.com/brave/brave-browser/blob/master/CHANGELOG_DESKTOP.md) in brave-browser master branch, must be completed before release to production Jenkins build job is run.
- [ ] Stage release notes to https://github.com/brave/brave-browser/releases/

### Certification and Builds
- [ ] Upload builds to Omaha test channels (`86-r-test`, `64-r-test`, `test`(mac))
- [ ] Log into Fastly, clear CDN cache for: `updates-cdn.bravesoftware.com`, `updates.bravesoftware.com`

### Test Staging for Updates
- [ ] Install a prior version of Brave and update via `test` channels (`86-r-test`, `64-r-test`, `test`(macOS))
   - [ ] Confirm version matches expectations
- [ ] QA summary and sign off report under #release via Slack.

### Release to production download locations
- [ ] Publish github release (remove 'pre-release' checkmark)
- [ ] Upload Mac/Win build to Omaha production channels (`x86-rel`, `x64-rel`, `stable`(mac))
- [ ] Sign Linux builds and upload to S3 repositories
- [ ] Upload Mac `.dmg` and `.pkg` to S3 bucket (i.e. `aws s3 cp ./Brave-Browser-Dev.dmg s3://brave-browser-downloads/latest/Brave-Browser-Dev.dmg --acl public-read`)
- [ ] Upload Windows stub and silent installer to S3 bucket using similar command to Mac dmg above (i.e `BraveBrowserSetup.exe`, `BraveBrowserSetup32.exe`, `BraveBrowserSilentSetup.exe`, `BraveBrowserSilentSetup32.exe`)

### Clear Production Fastly cache
- [ ] Clear Fastly cache for: `brave-browser-downloads.s3.brave.com`, `brave-browser-apt-release.s3.brave.com`, `brave-browser-rpm-release.s3.brave.com`

### Updates Testing on Production
- [ ] Wait for confirmation that Windows live update works
  - [ ] Ensure that `delta` upgrades are working on `Win x64` platforms
    - Save the [`configuration`](https://github.com/brave/brave-browser/wiki/(WIP)-Desktop-Release-Checklist#example-of-delta-upgrade-occurring) into a file named `BraveUpdate.ini` and move into `C:\`
    - Upgrade and you should see `C:\Omaha log.txt`. Confirm that a [`delta`](https://github.com/brave/brave-browser/wiki/(WIP)-Desktop-Release-Checklist#example-of-delta-upgrade-occurring) upgrade has occurred.
- [ ] Wait for confirmation that macOS live update works
- [ ] Wait for confirmation that Linux live update works

### Download & Install Binaries from Brave.com
- [ ] download binary from https://brave.com on `Win x64` - `https://laptop-updates.brave.com/latest/winx64`
- [ ] download binary from https://brave.com on `Win x86` - `https://laptop-updates.brave.com/latest/winia32`
- [ ] download binary from https://brave.com on `macOS` - `https://laptop-updates.brave.com/latest/osx`

### Announcements
- [ ] Publish the release notes to `GitHub`
- [ ] Publish the release notes to `https://www.brave.com/latest`
- [ ] Announce release on https://community.brave.com/ (@Hollons)
- [ ] Announce release on https://www.reddit.com/r/brave_browser/ (@Hollons)
- [ ] Notify #announcements of the latest release with a link to the release notes

### Closing milestones
- [ ] Set a release date (if missing) and close the appropriate milestone under https://github.com/brave/brave-browser/milestones
- [ ] Set a release date (if missing) and close the appropriate milestone under https://github.com/brave/brave-core/milestones