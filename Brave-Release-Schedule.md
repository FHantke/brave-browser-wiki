# Overview 

- Major Release frequency: ~3 weeks
- Minor / Chemspill frequency: As needed 
- Every 2nd release is tied to a fuzzy [Chromium release date](https://chromiumdash.appspot.com/schedule).

Older Brave Release Schedules are available in the [Brave Release Schedules Archive](https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule-Archive).

Information about channel differences available in the [Release Channel Descriptions](https://github.com/brave/brave-browser/wiki/Release-Channel-Descriptions).

# Current channel information:

| **Channel**     | Release |  Beta & Dev | Nightly|
| ----------------| ------- | ------ | ------ |
| **Milestone**   | 1.17.x  | 1.18.x    | 1.19.x |
| **Branch name** | 1.17.x | 1.18.x    | master |

Note: These versions represent which channel our CI builds things on. It may not reflect exactly the version on release channel for example. This would happen if for example Release above said `0.58.x` and it was in RC but on our website we still offered `0.57.x`.

# Current build schedule:

The following is our automated schedule for creating and uploading builds to our Sparkle (macOS), Omaha (Windows) servers and our Linux repositories.

| Channel | Time | Dates      | Public Build (Sparkle/Omaha)       |
|:-------:|:--------:|:---------------------:|:---------------------:|
| `Beta`| 4am UTC (9pm PST/12am EST) | Tuesday & Thursday | Yes (incl. Android and iOS framework)|
| `Dev`| 7am UTC (12am PST/3am EST)| Monday - Saturday | Yes|
| `Nightly`| 10am UTC (3am PST/6am EST)| Monday - Saturday| Yes (incl. Android and iOS framework)|
| `Nightly`| 6pm UTC (11am PST/2pm EST)| Monday - Friday| No (GitHub Only)|
| `Nightly`| 12am UTC (5pm PST/8pm EST)| Monday - Friday| No (GitHub Only)|
---

# Release channel dates:

| Version | Chromium | Migration Date      | Release Date        | Comments                                   |
|:-------:|:--------:|---------------------|---------------------|--------------------------------------------|
| 1.16.x  |    86    | October 20, 2020    | October 27, 2020    |                                            |
| 1.17.x  |    87    | November 10, 2020   | November 17, 2020   |                                            |
| 1.18.x  |    87    | December 1, 2020    | December 8, 2020    |                                            |
| 1.19.x  |    88    | January 12, 2021    | January 19, 2021    | Longer cycle/late release due to Holidays  |
| 1.20.x  |    88    | February 2, 2021    | February 9, 2021    |                                            |
| 1.21.x  |    89    | February 23, 2021   | March 2, 2021       |                                            |
| 1.22.x  |    89    | March 16, 2021      | March 23, 2021      |                                            |
| 1.23.x  |    90    | April 6, 2021       | April 13, 2021      |                                            |
| 1.24.x  |    90    | April 27, 2021      | May 4, 2021         |                                            |
| 1.25.x  |    91    | May 18, 2021        | May 25, 2021        |                                            |
| 1.26.x  |    91    | June 15, 2021       | June 22, 2021       | Longer cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.27.x  |    92    | July 13, 2021       | July 20, 2021       | Longer cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.28.x  |    92    | August 3, 2021      | August 10, 2021     |                                            |
| 1.29.x  |    93    | August 24, 2021     | August 31, 2021     |                                            |
| 1.30.x  |    93    | September 14, 2021  | September 21, 2021  |                                            |
| 1.31.x  |    94    | October 5, 2021     | October 12, 2021    |                                            |
| 1.32.x  |    94    | October 26, 2021    | November 2, 2021    |                                            |
| 1.33.x  |    95    | November 23, 2021   | November 30, 2021   | Longer cycle due to CR94 - CR95 being 49 days rather than 42 days|
| 1.34.x  |    95    | December 14, 2021   | December 21, 2021   |                                            |
| 1.35.x  |    96    | January 18, 2022    | January 25, 2022    | Longer cycle/late release due to Holidays  |

- All dates are approximate and are subject to change.
- CI does builds for release channel once a day if there are changes or as requested.
- Updates will be released as requested only.

### Number of days between each release on Release channel

| Version         | # of days between releases| Comments                                   |
|:---------------:|:-------------------------:|---------------------------------------------
| 1.15.x - 1.16.x | 20 days                   |                                            |
| 1.16.x - 1.17.x | 21 days                   |                                            |
| 1.17.x - 1.18.x | 21 days                   |                                            |
| 1.18.x - 1.19.x | 42 days                   | Longer cycle/late release due to Holidays  |
| 1.19.x - 1.20.x | 21 days                   |                                            |
| 1.20.x - 1.21.x | 21 days                   |                                            |
| 1.21.x - 1.22.x | 21 days                   |                                            |
| 1.22.x - 1.23.x | 21 days                   |                                            |
| 1.23.x - 1.24.x | 21 days                   |                                            |
| 1.24.x - 1.25.x | 21 days                   |                                            |
| 1.25.x - 1.26.x | 28 days                   | Longer cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.26.x - 1.27.x | 28 days                   | Longer cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.27.x - 1.28.x | 21 days                   |                                            |
| 1.28.x - 1.29.x | 21 days                   |                                            |
| 1.29.x - 1.30.x | 21 days                   |                                            |
| 1.30.x - 1.31.x | 21 days                   |                                            |
| 1.31.x - 1.32.x | 21 days                   |                                            |
| 1.32.x - 1.33.x | 28 days                   | Longer cycle due to CR94 - CR95 being 49 days rather than 42 days|
| 1.33.x - 1.34.x | 21 days                   |                                            |
| 1.34.x - 1.35.x | 35 days                   | Longer cycle/late release due to Holidays  |
---

# Beta & Dev channel dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.16.x  | 86                | September 29, 2020  |                                           |
| 1.17.x  | 87                | October 20, 2020    |                                           |
| 1.18.x  | 87                | November 10, 2020   |                                           |
| 1.19.x  | 88                | December 1, 2020    |                                           |
| 1.20.x  | 88                | January 12, 2021    | Longer cycle/late release due to Holidays |
| 1.21.x  | 89                | February 2, 2021    |                                           |
| 1.22.x  | 89                | February 23, 2021   |                                           |
| 1.23.x  | 90                | March 16, 2021      |                                           |
| 1.24.x  | 90                | April 6, 2021       |                                           |
| 1.25.x  | 91                | April 27, 2021      |                                           |
| 1.26.x  | 91                | May 18, 2021        | Longer cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.27.x  | 92                | June 15, 2021       | Longer cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.28.x  | 92                | July 13, 2021       |                                           |
| 1.29.x  | 93                | August 3, 2021      |                                           |
| 1.30.x  | 93                | August 24, 2021     |                                           |
| 1.31.x  | 94                | September 14, 2021  |                                           |
| 1.32.x  | 94                | October 5, 2021     |                                           |
| 1.33.x  | 95                | October 26, 2021    |                                           |
| 1.34.x  | 95                | November 23, 2021   | Longer cycle due to CR94 - CR95 being 49 days rather than 42 days|
| 1.35.x  | 96                | December 14, 2021   |                                           |
| 1.36.x  | 96                | January 18, 2022    | Longer cycle/late release due to Holidays |

- CI does builds for Beta channel happen twice a week.
- CI builds for Dev channel happen once a day on weekdays.
- Updates are released as there are builds as long as tests pass.

---

# Nightly channel dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.16.x  | 86                | September 8, 2020   |                                           |
| 1.17.x  | 87                | September 29, 2020  |                                           |
| 1.18.x  | 87                | October 20, 2020    |                                           |
| 1.19.x  | 88                | November 10, 2020   |                                           |
| 1.20.x  | 88                | December 1, 2020    |                                           |
| 1.21.x  | 89                | January 12, 2021    | Longer cycle/late release due to Holidays |
| 1.22.x  | 89                | February 2, 2021    |                                           |
| 1.23.x  | 90                | February 23, 2021   |                                           |
| 1.24.x  | 90                | March 16, 2021      |                                           |
| 1.25.x  | 91                | April 6, 2021       |                                           |
| 1.26.x  | 91                | April 27, 2021      |                                           |
| 1.27.x  | 92                | May 18, 2021        | Longer Cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.28.x  | 92                | June 15, 2021       | Longer Cycle due to CR91 - CR92 being 56 days rather than 42 days|
| 1.29.x  | 93                | July 13, 2021       |                                           |
| 1.30.x  | 93                | August 3, 2021      |                                           |
| 1.31.x  | 94                | August 24, 2021     |                                           |
| 1.32.x  | 94                | September 14, 2021  |                                           |
| 1.33.x  | 95                | October 5, 2021     |                                           |
| 1.34.x  | 95                | October 26, 2021    |                                           |
| 1.35.x  | 96                | November 23, 2021   | Longer cycle due to CR94 - CR95 being 49 days rather than 42 days|
| 1.36.x  | 96                | December 14, 2021   |                                           |
| 1.37.x  | 97                | January 18, 2022    | Longer cycle/late release due to Holidays |


- Nightly builds will be made from master for this channel.
- This means that within a day of any change you can start testing it.
- Developers work off of master.
- We keep master stable, when it is not, we backout what broke it.
- If something fails tests, that thing should be backed out.
- `brave/brave-browser`'s `package.json` specifies brave-core branch to be `master`.

# How to do branch migrations:

This process has recently been automated!
- Visit https://staging.ci.brave.com/job/branch-migrations/
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
