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
| **Milestone**   | 1.48.x  | 1.49.x    | 1.50.x |
| **Branch name** | 1.48.x | 1.49.x    | master |

Note: These versions represent which channel our CI builds things on. It may not reflect exactly the version on release channel for example. This would happen if for example Release above said `0.58.x` and it was in RC but on our website we still offered `0.57.x`.

# Current build schedule:

The following is our automated schedule for creating and uploading builds to our Sparkle (macOS), Omaha (Windows) servers and our Linux repositories.

| Channel | Time | Days      | Platforms | Public      |
|:-------:|:--------:|:---------------------:|:---------------------:|:---------------------:|
| `Beta`| 4am UTC (8pm PST/11pm EST) | Monday, Wednesday, Friday | android linux-x64 macos-x64 win64 | no |
| `Beta`| 4am UTC (8pm PST/11pm EST) | Tuesday, Thursday | all | yes |
| `Dev`| 8am UTC (12am PST/3am EST)| Tuesday, Thursday | linux-x64 macos-x64 macos-arm64 win64 win32 | yes |
| `Nightly`| 12pm UTC (4am PST/7am EST)| Monday - Friday| all | yes |
| `Nightly`| 4pm UTC (8am PST/11am EST)| Monday - Friday| android linux-x64 macos-x64 win64 | no |
| `Nightly`| 12am UTC (4pm PST/7pm EST)| Monday - Friday| android linux-x64 macos-x64 win64 | no |
---

# Release channel dates:

| Version | Chromium | Migration Date      | Release Date        | Comments                                   |
|:-------:|:--------:|---------------------|---------------------|--------------------------------------------|
| 1.44.x  |    106   | September 20, 2022  | September 27, 2022  |                                            |
| 1.44.x  |    106   | N/A                 | October 12, 2022    | Maintenance Release                        |
| 1.45.x  |    107   | October 18, 2022    | October 25, 2022    |                                            |
| 1.45.x  |    107   | N/A                 | November 15, 2022   | Maintenance Release.                       |
| 1.46.x  |    108   | November 22, 2022   | December 1, 2022    | Release delayed. Originally scheduled for `November 29, 2022`.|    
| 1.46.x  |    108   | N/A                 | December 14, 2022   | Maintenance Release                        |
| 1.46.x  |    108   | N/A                 | ~December 29, 2022~   | Maintenance Release (tentative) - `Note:` Depending on QA availability, this maintenance release may be moved to `January 4, 2023`. **Cancelled**.|
| 1.47.x  |    109   | January 4, 2023     | January 12, 2023    | Delayed due to Holiday PTO's. Originally scheduled for `January 10, 2023`|
| 1.47.x  |    109   | N/A                 | January 25, 2023    | Maintenance Release                        |
| 1.48.x  |    110   | February 1, 2023    | February 7, 2023    | Channel migrations delayed. Originally scheduled for `January 31, 2023`|
| 1.48.x  |    110   | N/A                 | February 22, 2023   | Maintenance Release                        |
| 1.49.x  |    111   | February 28, 2023   | March 7, 2023       |                                            |
| 1.49.x  |    111   | N/A                 | March 22, 2023      | Maintenance Release                        |
| 1.50.x  |    112   | March 28, 2023      | April 4, 2023       |                                            |
| 1.50.x  |    112   | N/A                 | April 19, 2023      | Maintenance Release                        |
| 1.51.x  |    113   | April 25, 2023      | May 2, 2023         |                                            |
| 1.51.x  |    113   | N/A                 | May 17, 2023        | Maintenance Release                        |
| 1.52.x  |    114   | May 23, 2023        | May 30, 2023        |                                            |
| 1.52.x  |    114   | N/A                 | June 14, 2023       | Maintenance Release                        |
| 1.53.x  |    115   | June 20, 2023       | June 27, 2023       |                                            |
| 1.53.x  |    115   | N/A                 | July 12, 2023       | Maintenance Release                        |
| 1.53.x  |    115   | N/A                 | July 26, 2023       | Maintenance Release                        |
| 1.54.x  |    116   | August 1, 2023      | August 8, 20203     |                                            |
| 1.54.x  |    116   | N/A                 | August 23, 2023     | Maintenance Release                        |
| 1.55.x  |    117   | August 29, 2023     | September 5, 2023   |                                            |
| 1.55.x  |    117   | N/A                 | September 20, 2023  | Maintenance Release                        |
| 1.56.x  |    118   | September 26, 2023  | October 3, 2023     |                                            |
| 1.56.x  |    118   | N/A                 | October 18, 2023    | Maintenance Release                        |
| 1.57.x  |    119   | October 24, 2023    | October 31, 2023    |                                            |

- All dates are approximate and are subject to change.
- `Maintenance releases` are tentative and subject to change. 
- `Migration Date` is also the cutoff date to add strings that need to be translated for the release. 
- Updates will be released as requested only.

### Number of days between each release on Release channel

| Version         | # of days between releases| Comments                                   |
|:---------------:|:-------------------------:|---------------------------------------------
| 1.44.x - 1.45.x | 28 days                   |                                            |
| 1.45.x - 1.46.x | 35 days                   |                                            |
| 1.46.x - 1.47.x | 42 days                   |                                            |
| 1.47.x - 1.48.x | 28 days                   |                                            |
| 1.48.x - 1.49.x | 28 days                   |                                            |
| 1.49.x - 1.50.x | 28 days                   |                                            |
| 1.50.x - 1.51.x | 28 days                   |                                            |
| 1.51.x - 1.52.x | 28 days                   |                                            |
| 1.52.x - 1.53.x | 28 days                   |                                            |
| 1.53.x - 1.54.x | 42 days                   |                                            |
| 1.54.x - 1.55.x | 28 days                   |                                            |
| 1.55.x - 1.56.x | 28 days                   |                                            |
| 1.56.x - 1.57.x | 28 days                   |                                            |
---

# Beta & Dev channel migration dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.44.x  | 106               | August 23, 2022     |                                           |
| 1.45.x  | 107               | September 20, 2022  |                                           |
| 1.46.x  | 108               | October 18, 2022    |                                           |
| 1.47.x  | 109               | November 22, 2022   |                                           |
| 1.48.x  | 110               | January 4, 2023     |                                           |
| 1.49.x  | 111               | January 31, 2023    |                                           |
| 1.50.x  | 112               | February 28, 2023   |                                           |
| 1.51.x  | 113               | March 28, 2023      |                                           |
| 1.52.x  | 114               | April 25, 2023      |                                           |
| 1.53.x  | 115               | May 23, 2023        |                                           |
| 1.54.x  | 116               | June 20, 2023       |                                           |
| 1.55.x  | 117               | August 1, 2023      |                                           |
| 1.56.x  | 118               | August 29, 2023     |                                           |
| 1.57.x  | 119               | September 26, 2023  |                                           |
| 1.58.x  | 120               | October 24, 2023    |                                           |

- CI does builds for Beta channel happen twice a week.
- CI builds for Dev channel happen once a day on weekdays.
- Updates are released as there are builds as long as tests pass.

---

# Nightly channel migration dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.45.x  | 107               | August 23, 2022     |                                           |
| 1.46.x  | 108               | September 20, 2022  |                                           |
| 1.47.x  | 109               | October 18, 2022    |                                           |
| 1.48.x  | 110               | November 22, 2022   |                                           |
| 1.49.x  | 111               | January 4, 2023     |                                           |
| 1.50.x  | 112               | January 31, 2023    |                                           |
| 1.51.x  | 113               | February 28, 2023   |                                           |
| 1.52.x  | 114               | March 28, 2023      |                                           |
| 1.53.x  | 115               | April 25, 2023      |                                           |
| 1.54.x  | 116               | May 23, 2023        |                                           |
| 1.55.x  | 117               | June 20, 2023       |                                           |
| 1.56.x  | 118               | August 1, 2023      |                                           |
| 1.57.x  | 119               | August 29, 2023     |                                           |
| 1.58.x  | 120               | September 26, 2023  |                                           |
| 1.59.x  | 121               | October 24, 2023    |                                           |

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