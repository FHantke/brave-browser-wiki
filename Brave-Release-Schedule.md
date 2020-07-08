# Overview 

- Major Release frequency: ~3 weeks
- Minor / Chemspill frequency: As needed 
- Every 2nd release is tied to a fuzzy [Chromium release date](https://chromiumdash.appspot.com/schedule).

Older Brave Release Schedules are available in the [Brave Release Schedules Archive](https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule-Archive).

Information about channel differences available in the [Release Channel Descriptions](https://github.com/brave/brave-browser/wiki/Release-Channel-Descriptions).

For iOS Release Schedule, please see https://docs.google.com/spreadsheets/d/14uGbc2rupJd0QbdBzqpUcivwLpix41siIR8mUP5lITs


# Current channel information:

| **Channel**     | Release |  Beta & Dev | Nightly|
| ----------------| ------- | ------ | ------ |
| **Milestone**   | 1.11.x  | 1.12.x    | 1.13.x |
| **Branch name** | 1.11.x | 1.12.x    | master |

Note: These versions represent which channel our CI builds things on. It may not reflect exactly the version on release channel for example. This would happen if for example Release above said `0.58.x` and it was in RC but on our website we still offered `0.57.x`.

# Current build schedule:

The following is our automated schedule for creating and uploading builds to our Sparkle (macOS), Omaha (Windows) servers and our Linux repositories.

| Channel | Time | Dates      | Public Build (Sparkle/Omaha)       |
|:-------:|:--------:|:---------------------:|:---------------------:|
| `Dev`| 4am UTC (9pm PST/12am EST) | Monday - Saturday| Yes|
| `Beta`| 7am UTC (12am PST/3am EST)| Tuesday & Thursday| Yes|
| `Nightly`| 10am UTC (3am PST/6am EST)| Monday - Saturday| Yes|
| `Nightly`| 6pm UTC (11am PST/2pm EST)| Monday - Friday| No (GitHub Only)|
| `Nightly`| 12am UTC (5pm PST/8pm EST)| Monday - Friday| No (GitHub Only)|

---

# Release channel dates:

| Version | Chromium | Migration Date      | Release Date        | Comments                                   |
|:-------:|:--------:|---------------------|---------------------|--------------------------------------------|
| 1.11.x  |    84    | July 7, 2020        | July, 14, 2020       |4 week cycle to keep in sync with Chromium  |
| 1.12.x  |    84    | July 28, 2020       | August 4, 2020      |4 week cycle to keep in sync with Chromium  |
| 1.13.x  |    85    | August 18, 2020     | August 25, 2020     |                                            |
| 1.14.x  |    85    | September 8, 2020   | September 15, 2020  |                                            |
| 1.15.x  |    86    | September 29, 2020  | October 6, 2020     |                                            |
| 1.16.x  |    86    | October 20, 2020    | October 27, 2020    |                                            |
| 1.17.x  |    87    | November 17, 2020   | November 24, 2020   |4 week cycle to keep in sync with Chromium  |
| 1.18.x  |    87    | December 8, 2020    | December 15, 2020   |                                            |

- All dates are approximate and are subject to change.
- CI does builds for release channel once a day if there are changes or as requested.
- Updates will be released as requested only.

### Number of days between each release on Release channel

| Version         | # of days between releases| Comments                                   |
|:---------------:|:-------------------------:|---------------------------------------------
| 1.4.x - 1.5.x   | 22 days                   |                                            |
| 1.5.x - 1.7.x   | 22 days                   |                                            |
| 1.7.x - 1.8.x   | 22 days                   |                                            |
| 1.8.x - 1.9.x   | 22 days                   |                                            |
| 1.9.x - 1.10.x  | 22 days                   |                                            |
| 1.10.x - 1.11.x | 29 days                   | 4 week cycle to keep in sync with Chromium |
| 1.11.x - 1.12.x | 29 days                   | 4 week cycle to keep in sync with Chromium |
| 1.12.x - 1.13.x | 22 days                   |                                            |
| 1.13.x - 1.14.x | 22 days                   |                                            |
| 1.14.x - 1.15.x | 22 days                   |                                            |
| 1.15.x - 1.16.x | 22 days                   |                                            |
| 1.16.x - 1.17.x | 29 days                   | 4 week cycle to keep in sync with Chromium |
| 1.17.x - 1.18.x | 22 days                   |                                            |

---

# Beta & Dev channel dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.4.x   | 80                | January 28, 2020    |                                           |
| 1.5.x   | 81                | February 18, 2020   |                                           |
| 1.7.x   | 81                | March 10, 2020      |                                           |
| 1.8.x   | 82                | March 31, 2020      |                                           |
| 1.9.x   | 82                | April 21, 2020      |                                           |
| 1.10.x   | 83               | May 12, 2020        |                                           |
| 1.11.x  | 83                | June 2, 2020        |                                           |
| 1.12.x  | 84                | July 7, 2020        |                                           |
| 1.13.x  | 84                | July 28, 2020       |                                           |
| 1.14.x  | 85                | August 18, 2020     |                                           |
| 1.15.x  | 85                | September 8, 2020   |                                           |
| 1.16.x  | 86                | September 29, 2020  |                                           |
| 1.17.x  | 86                | October 20, 2020    |                                           |
| 1.18.x  | 87                | November 17, 2020   |                                           |
| 1.19.x  | 87                | December 8, 2020    |                                           |

- CI does builds for Beta channel happen twice a week.
- CI builds for Dev channel happen once a day on weekdays.
- Updates are released as there are builds as long as tests pass.

---

# Nightly channel dates:

| Version | Target Chromium   | Migration Date      | Comments                                  |
|:-------:|:-----------------:|---------------------|-------------------------------------------|
| 1.4.x   | 80                | [December 3, 2019](https://ci.brave.com/job/branch-migrations/7) | |
| 1.5.x   | 80                | [December 30, 2019](https://ci.brave.com/job/branch-migrations/8) | |
| 1.6.x   | 80                | [February 4, 2020](https://ci.brave.com/job/branch-migrations/9/) | branch abandoned (beta+dev consolidated) |
| 1.7.x   | 81                | [February 19, 2020](https://ci.brave.com/job/branch-migrations/10/)| |
| 1.8.x   | 81                | [March 10, 2020](https://ci.brave.com/job/branch-migrations/13/) | |
| 1.9.x   | 83                | March 31, 2020      |                                           |
| 1.10.x  | 83                | April 21, 2020      |                                           |
| 1.11.x  | 84                | May 12, 2020        |                                           |
| 1.12.x  | 84                | June 2, 2020        |                                           |
| 1.13.x  | 85                | July 7, 2020        |                                           |
| 1.14.x  | 85                | July 28, 2020       |                                           |
| 1.15.x  | 86                | August 18, 2020     |                                           |
| 1.16.x  | 86                | September 8, 2020   |                                           |
| 1.17.x  | 87                | September 29, 2020  |                                           |
| 1.18.x  | 87                | October 20, 2020    |                                           |
| 1.19.x  | 88                | November 17, 2020   |                                           |
| 1.20.x  | 88                | December 8, 2020    |                                           |

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
