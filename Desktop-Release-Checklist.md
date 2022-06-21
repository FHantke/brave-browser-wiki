Feel free to copy this into an issue if you want to keep track of items per-milestone.
Be super sure that <version> is replaced with the version you wish to use.

### Prerequisites

- [ ] Double check the previous release/milestone and ensure everything has been QA'd
  - **Note:** If there's issues that need to be checked, create a `master` GH issue that lists the issues that need to be QA'd
- [ ] Ensure that the RC executable installs/launches without any issues on `x86` platforms (*Note:* preferably all three are checked but one should be good enough if others can't be checked) 
  - [ ] `Win 8.1 x86`
  - [ ] `Win 10 x86`
- [ ] Consult with the security team to ensure that all security issues have been included.
   - [ ] Release using the `critical` feature (forces users to update/restart) if there's a zero day vulnerability being exploited
- [ ] Consult with PR team (@catherinecorre) and provide heads up on release timing, screenshots, other deliverable's.

### Release Notes to Staging

- [ ] Mark closed issues in github as `release-notes/exclude` or `release-notes/include`.
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
  - [ ] Ensure that `delta` upgrades are working on both `Win x64` & `Win x86` platforms
    - Create the needed [`registry`](https://github.com/brave/brave-browser/wiki/Desktop-Release-Checklist/_edit#enabling-logging-to-check-delta-upgrades-on-win-x64--win-x86) keys
    - Upgrade and you should see `C:\ProgramData\BraveSoftware\Update\Log\BraveUpdate.log`. Confirm that a [`x64 delta`](https://github.com/brave/brave-browser/wiki/Desktop-Release-Checklist/_edit#x64-example) or [`x86 delta`](https://github.com/brave/brave-browser/wiki/Desktop-Release-Checklist/_edit#x86-example) upgrade has occurred.
- [ ] Wait for confirmation that macOS live update works
- [ ] Wait for confirmation that Linux live update works
  - [ ] ensure that following https://brave.com/linux downloads/installs the expected release (version we just published/released)

### Download & Install stub binaries from https://brave.com

- [ ] download the stub installer from https://brave.com on `Win x64` - `https://laptop-updates.brave.com/latest/winx64`
- [ ] download the stub installer from https://brave.com on `Win x86` - `https://laptop-updates.brave.com/latest/winia32`
- [ ] download the `.dmg` binary from https://brave.com on `macOS` - `https://laptop-updates.brave.com/latest/osx`

### Announcements

- [ ] Publish the release notes to `GitHub`
- [ ] Publish the release notes to `https://www.brave.com/latest`
- [ ] Announce release on https://community.brave.com/ (@Hollons)
- [ ] Announce release on https://www.reddit.com/r/brave_browser/ (@Hollons)
- [ ] Notify #announcements of the latest release with a link to the release notes

Additional announcements for 0-day and critical releases 
- [ ] Announce release on Twitter `https://twitter.com/brave` (@rebron)
- [ ] Announce release on Facebook `https://www.facebook.com/BraveSoftware` (@rebron)

### Closing milestones

- [ ] Set a release date (if missing) and close the appropriate milestone under https://github.com/brave/brave-browser/milestones
- [ ] Set a release date (if missing) and close the appropriate milestone under https://github.com/brave/brave-core/milestones

### Creating milestones

- [ ] Create milestones under [`b-b`](https://github.com/brave/brave-browser/milestones) & [`b-c`](https://github.com/brave/brave-core/milestones) if needed

---------------------------

### Enabling Logging to check delta upgrades on `Win x64` & `Win x86`

Once Brave is installed and you're ready to run through an upgrade, the following registry keys need to be added using `Powershell`:

#### `Win x64`

```
New-Item –Path "HKLM:\SOFTWARE\WOW6432Node\BraveSoftware" –Name UpdateDev
New-ItemProperty -Path "HKLM:\SOFTWARE\WOW6432Node\BraveSoftware\UpdateDev" -Name "IsEnabledLogToFile" -Value ”1”  -PropertyType "DWord"
```

#### `Win x86`

```
New-Item –Path "HKLM:\SOFTWARE\BraveSoftware" –Name UpdateDev
New-ItemProperty -Path "HKLM:\SOFTWARE\BraveSoftware\UpdateDev" -Name "IsEnabledLogToFile" -Value ”1”  -PropertyType "DWord"
```

**Note:** You'll need to run Powershell as an `Administrator` or you'll get an error message similar to the following:

```
PS C:\Users\kamil> New-Item –Path "HKLM:\SOFTWARE\WOW6432Node\BraveSoftware" –Name UpdateDev
New-Item : Requested registry access is not allowed.
At line:1 char:1
+ New-Item –Path "HKLM:\SOFTWARE\WOW6432Node\BraveSoftware" –Name Updat ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (HKEY_LOCAL_MACH...e\BraveSoftware:String) [New-Item], SecurityExcepti
   on
    + FullyQualifiedErrorId : System.Security.SecurityException,Microsoft.PowerShell.Commands.NewItemCommand
```

Once you've created the above registry keys, update Brave via `brave://settings/help` and you should see a log file created under the following directory :

```
C:\ProgramData\BraveSoftware\Update\Log\
```

You'll see something similar to the following under `BraveUpdate.log` which indicates that the `brave_installer-delta-x64.exe` was used for the upgrade:

#### `x64 example`

```[02/22/22 19:48:16.672][BraveUpdate:goopdate][2304:3864][Running installer][C:\Program Files (x86)\BraveSoftware\Update\Install\{EA78FFA4-26E6-459D-9D48-03CA7B5750EE}\brave_installer-delta-x64.exe][][{AFE6A462-C574-4B8A-AF43-4CC60DF4563B}]```

#### `x86 example`

```[02/22/22 20:04:57.790][BraveUpdate:goopdate][9384:9404][Running installer][C:\Program Files\BraveSoftware\Update\Install\{0700DFFF-3AD1-406E-9622-B46E1D93C408}\brave_installer-delta-ia32.exe][][{AFE6A462-C574-4B8A-AF43-4CC60DF4563B}]```