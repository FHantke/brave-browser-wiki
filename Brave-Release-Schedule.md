# TOC
- [Overview](#overview)
- [Current channel information](#current-channel-information)
- [Current build schedule](#current-build-schedule)
- [Release channel dates](#release-channel-dates)
    - [Number of days between each release on Release channel](#number-of-days-between-each-release-on-release-channel)
- [Beta & Dev channel migration dates](#beta--dev-channel-migration-dates)
- [Nightly channel migration dates](#nightly-channel-migration-dates)
- [Mobile platforms]()
    - [Android rollout process](#android-rollout-process)
    - [iOS rollout process](#ios-rollout-process)
- [How to do branch migrations](#how-to-do-branch-migrations)

# Overview 

Currently published version numbers can be seen at https://brave-browser-downloads.s3.brave.com/

- Major Release frequency: ~4 weeks
- Maintenance Release frequency: ~2 weeks after a major release (however, these are tentative and subject to change)
- Chemspill/Hotfix frequency: As needed 
- Every major release is tied to a fuzzy [Chromium release date](https://chromiumdash.appspot.com/schedule).

Older Brave Release Schedules are available in the [Brave Release Schedules Archive](https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule-Archive).

Information about channel differences available in the [Release Channel Descriptions](https://github.com/brave/brave-browser/wiki/Release-Channel-Descriptions).

# Current channel information:

| **Channel**     | Release |  Beta & Dev | Nightly|
| ----------------| ------- | ------ | ------ |
| **Milestone**   | 1.58.x  | 1.59.x    | 1.60.x |
| **Branch name** | 1.58.x | 1.59.x    | master |

Note: These versions represent which channel our CI builds things on. It may not reflect exactly the version on release channel for example. This would happen if for example Release above said `0.58.x` and it was in RC but on our website we still offered `0.57.x`.

# Current build schedule:

The following is our automated schedule for creating and uploading builds to our Sparkle (macOS), Omaha (Windows) servers and our Linux repositories.

| Channel | Time | Days      | Platforms | Public      |
|:-------:|:--------:|:---------------------:|:---------------------:|:---------------------:|
| `Beta`| 4am UTC (8pm PST/11pm EST) | Monday, Wednesday, Friday | android linux-x64 macos-x64 macos-arm64 windows-x64 | no |
| `Beta`| 4am UTC (8pm PST/11pm EST) | Tuesday, Thursday | all | yes |
| `Dev`| 8am UTC (12am PST/3am EST)| Saturday | linux-x64 linux-arm64 macos-x64 macos-arm64 windows-x64 windows-x86 windows-arm64 | yes |
| `Nightly`| 12pm UTC (4am PST/7am EST)| Monday - Friday| all | yes |
| `Nightly`| 4pm UTC (8am PST/11am EST)| Monday - Friday| android linux-x64 macos-x64 macos-arm64 windows-x64 | no |
| `Nightly`| 12am UTC (4pm PST/7pm EST)| Monday - Friday| android linux-x64 macos-x64 macos-arm64 windows-x64 | no |
---

# Release channel dates:

| Version | Chromium | Migration Date      | Release Date        | Comments                                   |
|:-------:|:--------:|---------------------|---------------------|--------------------------------------------|
| 1.58.x  |    117   | September 6, 2023   | September 14, 2023  | Release delayed. Originally scheduled for `September 12, 2023`. |             
| 1.58.x  |    117   | N/A                 | September 27, 2023  | Maintenance Release                        |
| 1.59.x  |    118   | October 4, 2023     | October 10, 2023    |                                            |
| 1.60.x  |    119   | October 25, 2023    | October 31, 2023    |                                            |
| 1.60.x  |    119   | N/A                 | November 15, 2023   | Maintenance Release                        |
| 1.61.x  |    120   | November 29, 2023   | December 5, 2023    |                                            |
| 1.62.x  |    121   | January 17, 2024    | January 23, 2024    |                                            |
| 1.63.x  |    122   | February 14, 2024   | February 20, 2024   |                                            |
| 1.64.x  |    123   | March 13, 2024      | March 19, 2024      |                                            |
| 1.65.x  |    124   | April 10, 2024      | April 16, 2024      |                                            |
| 1.66.x  |    125   | May 8, 2024         | May 14, 2024        |                                            |
| 1.67.x  |    126   | June 5, 2024        | June 11, 2024       |                                            |
| 1.68.x  |    127   | July 17, 2024       | July 23, 2024       |                                            |
| 1.69.x  |    128   | August 14, 2024     | August 20, 2024     |                                            |
| 1.70.x  |    129   | September 11, 2024  | September 17, 2024  |                                            |
| 1.71.x  |    130   | October 9, 2024     | October 15, 2024    |                                            |
| 1.72.x  |    131   | November 6, 2024    | November 12, 2024   |                                            |

**`Note:`** As announced via https://security.googleblog.com/2023/08/an-update-on-chrome-security-updates.html, we'll be getting minor chromium bumps every Tuesday. These releases will sometime also include `P1` & `P2` issues that are deemed important enough to get out ASAP rather than waiting for a major release. For example, HackerOne reports or other issues that are affecting users in a major way.

- All dates are approximate and are subject to change.
- `Maintenance releases` are tentative and subject to change. 
- `Migration Date` is also the cutoff date to add strings that need to be translated for the release. 
- Updates will be released as requested only.

### Number of days between each release on Release channel

| Version         | # of days between releases| Comments                                   |
|:---------------:|:-------------------------:|---------------------------------------------
| 1.58.x - 1.59.x | 29 days                   |                                            |
| 1.59.x - 1.60.x | 21 days                   |                                            |
| 1.60.x - 1.61.x | 35 days                   |                                            |
| 1.61.x - 1.62.x | 49 days                   |                                            |
| 1.62.x - 1.63.x | 28 days                   |                                            |
| 1.63.x - 1.64.x | 28 days                   |                                            |
| 1.64.x - 1.65.x | 28 days                   |                                            |
| 1.65.x - 1.66.x | 28 days                   |                                            |
| 1.66.x - 1.67.x | 28 days                   |                                            |
| 1.67.x - 1.68.x | 42 days                   |                                            |
| 1.68.x - 1.69.x | 28 days                   |                                            |
| 1.69.x - 1.70.x | 28 days                   |                                            |
| 1.70.x - 1.71.x | 28 days                   |                                            |
| 1.71.x - 1.72.x | 28 days                   |                                            |
---

# Beta & Dev channel migration dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.60.x  | 119               | October 4, 2023     |                                           |
| 1.61.x  | 120               | October 25, 2023    |                                           |
| 1.62.x  | 121               | November 29, 2023   |                                           |
| 1.63.x  | 122               | January 17, 2024    |                                           |
| 1.64.x  | 123               | February 14, 2024   |                                           |
| 1.65.x  | 124               | March 13, 2024      |                                           |
| 1.66.x  | 125               | April 10, 2024      |                                           |
| 1.67.x  | 126               | May 8, 2024         |                                           |
| 1.68.x  | 127               | June 5, 2024        |                                           |
| 1.69.x  | 128               | July 17, 2024       |                                           |
| 1.70.x  | 129               | August 14, 2024     |                                           |
| 1.71.x  | 130               | September 11, 2024  |                                           |
| 1.72.x  | 131               | October 9, 2024     |                                           |
| 1.73.x  | 132               | November 6, 2024    |                                           |

- CI does builds for Beta channel happen twice a week.
- CI builds for Dev channel happen once a day on weekdays.
- Updates are released as there are builds as long as tests pass.

---

# Nightly channel migration dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.52.x  | 114               | March 28, 2023      |                                           |
| 1.56.x  | 115               | April 25, 2023      |                                           |
| 1.57.x  | 116               | May 23, 2023        |                                           |
| 1.58.x  | 117               | June 20, 2023       |                                           |
| 1.59.x  | 118               | August 8, 2023      |                                           |
| 1.60.x  | 119               | September 5, 2023   |                                           |
| 1.61.x  | 120               | October 3, 2023     |                                           |
| 1.62.x  | 121               | October 24, 2023    |                                           |

- Nightly builds will be made from master for this channel.
- This means that within a day of any change you can start testing it.
- Developers work off of master.
- We keep master stable, when it is not, we backout what broke it.
- If something fails tests, that thing should be backed out.
- `brave/brave-browser`'s `package.json` specifies brave-core branch to be `master`.

# Mobile platforms
## Android rollout process
When 1) approved in the Google Play Store and 2) QA has signed off on the release, we'll release in a staged rollout. Before moving to the next stage, crash reports are checked by release team and evaluated.

Assuming we have the release ready in the morning, the schedule will typically look like this:
| day | time | rollout             |
|:---:|:----:|---------------------|
| 1 | morning | initial release 1% |
| 1 | evening | 5%                 |
| 2 | morning | 10%                |
| 2 | evening | 20%                |
| 3 | morning | 50%                |
| 3 | evening | 100%               |

## iOS rollout process
When 1) approved in iOS App Store and 2) QA has signed off on the release, we'll begin the automated rollout. This takes place over 7 days to people who have automatic updates enabled. Users can visit the App Store page for Brave at any time and get the latest one

If something goes wrong, the release can be paused.

| day | rollout             |
|:---:|---------------------|
| 1 | initial release 1% |
| 2 | 2% |
| 3 | 5% |
| 4 | 10% |
| 5 | 20% |
| 6 | 50% |
| 7 | 100% |

# How to do branch migrations:

- Visit https://ci.brave.com/job/branch-migrations/
- Queue the job
- Verify results

For folks that want to see more of "how the sausage is made" (link restricted to Brave employees), you can view the Jenkins job details here:
https://github.com/brave/devops/blob/master/jenkins/jobs/browser/branch-migrations.yml