# Overview 

- Major Release frequency: ~4 weeks
- Maintenance Release frequency: ~2 weeks after a major release (however, these are tentative and subject to change)
- Chemspill/Hotfix frequency: As needed 
- Every major release is tied to a fuzzy [Chromium release date](https://chromiumdash.appspot.com/schedule).

Older Brave Release Schedules are available in the [Brave Release Schedules Archive](https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule-Archive).

Information about channel differences available in the [Release Channel Descriptions](https://github.com/brave/brave-browser/wiki/Release-Channel-Descriptions).

# Current channel information:

| **Channel**     | Release |  Beta & Dev | Nightly|
| ----------------| ------- | ------ | ------ |
| **Milestone**   | 1.38.x  | 1.39.x    | 1.40.x |
| **Branch name** | 1.38.x | 1.39.x    | master |

Note: These versions represent which channel our CI builds things on. It may not reflect exactly the version on release channel for example. This would happen if for example Release above said `0.58.x` and it was in RC but on our website we still offered `0.57.x`.

# Current build schedule:

The following is our automated schedule for creating and uploading builds to our Sparkle (macOS), Omaha (Windows) servers and our Linux repositories.

| Channel | Time | Days      | Platforms | Public      |
|:-------:|:--------:|:---------------------:|:---------------------:|:---------------------:|
| `Beta`| 4am UTC (8pm PST/11pm EST) | Monday, Wednesday, Friday | android linux-x64 macos-x64 win64 | no |
| `Beta`| 4am UTC (8pm PST/11pm EST) | Tuesday, Thursday | all | yes |
| `Dev`| 8am UTC (12am PST/3am EST)| Tuesday, Thursday | linux-x64 macos-x64 macos-arm64 win64 win32 | yes |
| `Nightly`| 12pm UTC (4am PST/7am EST)| Monday - Friday| all | yes |
| `Nightly`| 6pm UTC (10am PST/1pm EST)| Monday - Friday| android linux-x64 macos-x64 win64 | no |
| `Nightly`| 12am UTC (4pm PST/7pm EST)| Monday - Friday| android linux-x64 macos-x64 win64 | no |
---

# Release channel dates:

| Version | Chromium | Migration Date      | Release Date        | Comments                                   |
|:-------:|:--------:|---------------------|---------------------|--------------------------------------------|
| 1.35.x  |    98    | January 26, 2022    | February 2, 2022    | Migration delayed. Originally set as `January 25, 2021`.  Release delayed. Originally set as `February 1, 2022`|
| 1.36.x  |    99    | February 23, 2022   | March 2, 2022       | Migration delayed due to Holiday. Originally set as `February 22, 2021`.  Release delayed. Originally set as `March 1, 2022`|
| 1.36.x  |    99    | N/A                 | March 16, 2022      | Maintenance Release                        |
| 1.37.x  |    100   | March 22, 2022      | March 30, 2022      |Release delayed. Originally set as `March 29, 2022`|    
| 1.37.x  |    100   | N/A                 | April 14, 2022      | Maintenance Release                        |
| 1.38.x  |    101   | April 20, 2022      | April 27, 2022      | Migration delayed. Originally set as `April 19, 2022`. Release delayed. Originally set as `April 26, 2022`|
| 1.38.x  |    101   | N/A                 | May 17, 2022        | Maintenance Release. Release delayed. Originally set as `May 11, 2022`|
| 1.39.x  |    102   | May 17, 2022        | May 24, 2022        |                                            |
| 1.39.x  |    102   | N/A                 | June 8, 2022        | Maintenance Release                        |
| 1.40.x  |    103   | June 14, 2022       | June 21, 2022       |                                            |
| 1.41.x  |    103   | July 5, 2022        | July 12, 2022       | Shorter cycle due to C103 - C104 being 42 days rather than 28 days|
| 1.42.x  |    104   | July 26, 2022       | August 2, 2022      | Shorter cycle due to C103 - C104 being 42 days rather than 28 days|
| 1.42.x  |    104   | N/A                 | August 17, 2022     | Maintenance Release                        |
| 1.43.x  |    105   | August 23, 2022     | August 30, 2022     |                                            |
| 1.43.x  |    105   | N/A                 | September 14, 2022  | Maintenance Release                        |
| 1.44.x  |    106   | September 20, 2022  | September 27, 2022  |                                            |
| 1.44.x  |    106   | N/A                 | October 12, 2022    | Maintenance Release                        |
| 1.45.x  |    107   | October 18, 2022    | October 25, 2022    |                                            |

- All dates are approximate and are subject to change.
- `Maintenance releases` are tentative and subject to change. 
- `Migration Date` is also the cutoff date to add strings that need to be translated for the release. 
- Updates will be released as requested only.

### Number of days between each release on Release channel

| Version         | # of days between releases| Comments                                   |
|:---------------:|:-------------------------:|---------------------------------------------
| 1.26.x - 1.27.x | 28 days                   | Longer cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.27.x - 1.28.x | 21 days                   |                                            |
| 1.28.x - 1.29.x | 21 days                   |                                            |
| 1.29.x - 1.30.x | 21 days                   |                                            |
| 1.30.x - 1.31.x | 28 days                   |                                            |
| 1.31.x - 1.32.x | 28 days                   |                                            |
| 1.32.x - 1.33.x | 21 days                   | Shorter cycle due to C96 - C97 being 49 days rather than 28 days|
| 1.33.x - 1.34.x | 28 days                   |                                            |
| 1.34.x - 1.35.x | 28 days                   |                                            |
| 1.35.x - 1.36.x | 28 days                   |                                            |
| 1.36.x - 1.37.x | 28 days                   |                                            |
| 1.37.x - 1.38.x | 28 days                   |                                            |
| 1.38.x - 1.39.x | 28 days                   |                                            |
| 1.39.x - 1.40.x | 28 days                   |                                            |
| 1.40.x - 1.41.x | 21 days                   | Shorter cycle due to C103 - C104 being 42 days rather than 28 days|
| 1.41.x - 1.42.x | 21 days                   | Shorter cycle due to C103 - C104 being 42 days rather than 28 days|
| 1.42.x - 1.43.x | 28 days                   |                                            |
| 1.43.x - 1.44.x | 28 days                   |                                            |
| 1.44.x - 1.45.x | 28 days                   |                                            |
---

# Beta & Dev channel dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.27.x  | 92                | June 15, 2021       | Longer cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.28.x  | 92                | July 14, 2021       |                                           |
| 1.29.x  | 93                | August 4, 2021      |                                           |
| 1.30.x  | 94                | August 24, 2021     |                                           |
| 1.31.x  | 95                | September 17, 2021  |Migration delayed. Originally set as `September 14, 2021`|
| 1.32.x  | 96                | October 12, 2021    |                                           |
| 1.33.x  | 96                | November 9, 2021    |                                           |
| 1.34.x  | 97                | November 30, 2021   | Shorter cycle due to C96 - C97 being 49 days rather than 28 days|
| 1.35.x  | 98                | December 28, 2021   |                                           |
| 1.36.x  | 99                | January 25, 2022    |                                           |
| 1.37.x  | 100               | February 22, 2022   |                                           |
| 1.38.x  | 101               | March 22, 2022      |                                           |
| 1.39.x  | 102               | April 19, 2022      |                                           |
| 1.40.x  | 103               | May 17, 2022        |                                           |
| 1.41.x  | 103               | June 14, 2022       |                                           |
| 1.42.x  | 104               | July 5, 2022        | Shorter cycle due to C103 - C104 being 42 days rather than 28 days|
| 1.43.x  | 105               | July 26, 2022       | Shorter cycle due to C103 - C104 being 42 days rather than 28 days|
| 1.44.x  | 106               | August 23, 2022     |                                           |
| 1.45.x  | 107               | September 20, 2022  |                                           |
| 1.46.x  | 108               | October 18, 2022    |                                           |

- CI does builds for Beta channel happen twice a week.
- CI builds for Dev channel happen once a day on weekdays.
- Updates are released as there are builds as long as tests pass.

---

# Nightly channel dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.27.x  | 92                | May 18, 2021        | Longer Cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.28.x  | 92                | June 15, 2021       | Longer Cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.29.x  | 93                | July 14, 2021       |                                           |
| 1.30.x  | 93                | August 4, 2021      |                                           |
| 1.31.x  | 95                | August 24, 2021     |                                           |
| 1.32.x  | 96                | September 17, 2021  |Migration delayed. Originally set as `September 14, 2021`|
| 1.33.x  | 96                | October 12, 2021    |                                           |
| 1.34.x  | 97                | November 9, 2021    |                                           |
| 1.35.x  | 98                | November 30, 2021   | Longer cycle due to CR94 - CR95 being 49 days rather than 42 days|
| 1.36.x  | 99                | December 28, 2021   |                                           |
| 1.37.x  | 100               | January 25, 2022    |                                           |
| 1.38.x  | 101               | February 22, 2022   |                                           |
| 1.39.x  | 102               | March 22, 2022      |                                           |
| 1.40.x  | 103               | April 19, 2022      |                                           |
| 1.41.x  | 103               | May 17, 2022        |                                           |
| 1.42.x  | 104               | June 14, 2022       |                                           |
| 1.43.x  | 105               | July 5, 2022        | Shorter cycle due to C103 & C104 being 42 days rather than 28 days|
| 1.44.x  | 106               | July 26, 2022       | Shorter cycle due to C103 & C104 being 42 days rather than 28 days|
| 1.45.x  | 107               | August 23, 2022     |                                           |
| 1.46.x  | 108               | September 20, 2022  |                                           |
| 1.47.x  | 109               | October 18, 2022    |                                           |

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

# How to do branch migrations:

- Visit https://ci.brave.com/job/branch-migrations/
- Queue the job
- Verify results

## Manual process (for reference)

Follow this checklist:

- [ ] Create a new branch off of `master` of `brave/brave-core` for the new version. `cd src/brave && git checkout master && git pull`
- [ ] Create a new branch off of `master` of `brave/brave-browser` for the new version.  `git checkout master && git pull`
- [ ] Create a new branch in the `brave/brave-ui` repo, off of the sha referenced in brave-core's package.json for the brave-ui dependency (could be different to master), named `brave-core-Z.YY.x`
- [ ] Update info in the chart at the top of https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule 
- [ ] Verify branch protection is still in effect for `brave/brave-browser` and `brave/brave-core` for the new version. If it is of the format `0.*.x` then it will be.
- [ ] Rename milestone names with the channel name on brave-browser.  Create a new one for the new Nightly.
- [ ] Rename milestone names with the channel name on brave-core. Create a new one for the new Nightly.
- [ ] Update versions on brave-browser with a PR like this: https://github.com/brave/brave-browser/commit/2a118fc38cc6357c15678db37a57dfd030328877
- [ ] Update the version for brave-core in package.json to the branch, similar to: https://github.com/brave/brave-browser/blame/0.59.x/package.json#L45
- [ ] Update versions on brave-core with a commit like this: https://github.com/brave/brave-core/commit/f65158c72d65f83194a72e5b6386693e61b48c8c
- [ ] Message everyone in slack in #brave-core about each branch and where it lives.
