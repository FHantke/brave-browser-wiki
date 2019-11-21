# Overview 

- Major Release frequency: ~3 weeks
- Minor / Chemspill frequency: As needed 
- Every 2nd release is tied to a fuzzy [Chromium release date](https://www.chromium.org/developers/calendar).

Older Brave Release Schedules are available in the [Brave Release Schedules Archive](https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule-Archive).
Information about channel differences available in the [Release Channel Descriptions](https://github.com/brave/brave-browser/wiki/Release-Channel-Descriptions).

# Current channel information:

| **Channel**     | Release |  Beta  | Dev       | Nightly|
| ----------------| ------- | ------ | --------- | ------ |
| **Milestone**   | 1.0.x   | 1.1.x  | 1.2.x     | 1.3.x  |
| **Branch name** | 1.0.x   | 1.1.x  | 1.2.x     | master |

Note: These versions represent which channel our CI builds things on. It may not reflect exactly the version on release channel for example. This would happen if for example Release above said `0.58.x` and it was in RC but on our website we still offered `0.57.x`.

# Freeze

Freeze time is defined to be a period of time where only bug fixes, Chromium minor upgrades, security issues, and privacy issues are accepted. Nothing that smells like a feature can be [uplifted](https://github.com/brave/brave-browser/wiki/Triage-Guidelines#uplift-approvers) in these periods of time.  Text changes are also not accepted unless it's a very bad typo that we missed.  This is important for stability reasons, localization, and making sure there's time to QA and dogfood changes.

If a feature being worked on just misses the freeze time, then it will land on the next branch which is 1 release cycle higher.  This happens even if the feature was originally wanted for a certain version.

Release, Beta, and Dev channels are considered to be Frozen.

---

# Release channel dates:

| Version | Chromium | Migration Date      | Release Date        | Comments                           |
|:-------:|:--------:|---------------------|---------------------| ------------------------------------|
| 1.1.x   |    79    | December 3, 2019    | December 10, 2019   |                                     |
| 1.2.x   |    79    | December 30, 2019   | January 6, 2020     | Late release due to holidays        |
| 1.3.x   |    80    | January 28, 2020    | February 4, 2020    |                                     |
| 1.4.x   |    80    | February 18, 2020   | February 25, 2020   |                                     |
| 1.5.x   |    81    | March 10, 2020      | March 17, 2020      |                                     |
| 1.6.x   |    81    | March 31, 2020      | April 7, 2020       |                                     |
| 1.7.x   |    82    | April 21, 2020      | April 28, 2020      |                                     |
| 1.8.x   |    82    | May 12, 2020        | May 19, 2020        |                                     |
| 1.9.x   |    83    | June 2, 2020        | June 9, 2020        |                                     |
| 1.10.x  |    83    | June 30, 2020       | July, 7, 2020       | 4 week cycle to sync with CR        |
| 1.11.x  |    84    | July 28, 2020       | August 4, 2020      | 4 week cycle to sync with CR        |
| 1.12.x  |    84    | August 18, 2020     | August 25, 2020     |                                     |
| 1.13.x  |    85    | September 8, 2020   | September 15, 2020  |                                     |
| 1.14.x  |    85    | September 29, 2020  | October 6, 2020     |                                     |
| 1.15.x  |    86    | October 20, 2020    | October 27, 2020    |                                     |
| 1.16.x  |    86    | November 17, 2020   | November 24, 2020   | 4 week cycle to sync with CR        |
| 1.17.x  |    87    | December 8, 2020    | December 15, 2020   |                                     |

- All dates are approximate and are subject to change.
- CI does builds for release channel once a day if there are changes or as requested.
- Updates will be released as requested only.

### Number of days between each release on Release channel


| Version         | # of days between releases| Comments                                   |
|-----------------|---------------------------|---------------------------------------------
| 1.2.x - 1.3.x   | 30 days                   | 4 week cycle due to holidays               |
| 1.3.x - 1.4.x   | 22 days                   |                                            |
| 1.4.x - 1.5.x   | 22 days                   |                                            |
| 1.5.x - 1.6.x   | 22 days                   |                                            |
| 1.6.x - 1.7.x   | 22 days                   |                                            |
| 1.7.x - 1.8.x   | 22 days                   |                                            |
| 1.8.x - 1.9.x   | 22 days                   |                                            |
| 1.9.x - 1.10.x  | 29 days                   | 4 week cycle to keep in sync with Chromium |
| 1.10.x - 1.11.x | 29 days                   | 4 week cycle to keep in sync with Chromium |
| 1.11.x - 1.12.x | 22 days                   |                                            |
| 1.12.x - 1.13.x | 22 days                   |                                            |
| 1.13.x - 1.14.x | 22 days                   |                                            |
| 1.14.x - 1.15.x | 22 days                   |                                            |
| 1.15.x - 1.16.x | 29 days                   | 4 week cycle to keep in sync with Chromium |
| 1.16.x - 1.17.x | 22 days                   |                                            |

---

# Beta channel dates:

| Version | Chromium | Migration Date     | Comments                                  |
| ------- | ---------|--------------------|-------------------------------------------|
| 0.62.x  |    73    | March 5, 2019      |  
| 0.63.x  |    74    | March 26, 2019     |              
| 0.64.x  |    74    | April 16, 2019     |              
| 0.65.x  |    75    | May 7, 2019        |              
| 0.66.x  |    75    | May 28, 2019       |              
| 0.67.x  |    76    | June 25, 2019      |              
| 0.68.x  |    76    | July 23, 2019      |              
| 0.69.x  |    77    | August 13, 2019    |              
| 0.70.x  |    77    | September 4, 2019  |              
| 0.71.x  |    78    | October 15, 2019   |              
| 0.72.x  |    79    | November 5, 2019   |              
| 0.73.x  |    79    | December 3, 2019   |              

- CI does builds for Beta channel once a day if there are changes or as requested.
- Updates are released as there are builds as long as tests pass.

---

# Dev channel dates:

| Version | Target Chromium | Migration Date     | Comments                                  |
| ------- | ----------------|--------------------|-------------------------------------------|
| 0.63.x  |    74           | March 5, 2019      |                     
| 0.64.x  |    74           | March 26, 2019     |              
| 0.65.x  |    75           | April 16, 2019     |                     
| 0.66.x  |    75           | May 7, 2019        |              
| 0.67.x  |    76           | May 28, 2019       |                     
| 0.68.x  |    76           | June 25, 2019      |              
| 0.69.x  |    77           | July 23, 2019      |                     
| 0.70.x  |    77           | August 13, 2019    |              
| 0.71.x  |    78           | September 4, 2019  |                     
| 0.72.x  |    79           | October 15, 2019   | 
| 0.73.x  |    79           | November 5, 2019   |              
| 0.74.x  |    80           | December 3, 2019   |              

- CI does builds for Dev channel once a day if there are changes or as requested.
- Updates are released as there are builds as long as tests pass.
- Branches are created from master around the dates specified in this table.

---

# Nightly channel dates:

| Version | Target Chromium | Migration Date     | Comments                                  |
| ------- | ----------------|--------------------|-------------------------------------------|
| 0.65.x  |    75           | March 26, 2019     |              
| 0.66.x  |    75           | April 16, 2019     |                     
| 0.67.x  |    76           | May 7, 2019        |              
| 0.68.x  |    76           | May 28, 2019       |                     
| 0.69.x  |    77           | June 25, 2019      |              
| 0.70.x  |    77           | July 23, 2019      |                     
| 0.71.x  |    78           | August 13, 2019    |              
| 0.72.x  |    78           | September 4, 2019  |                     
| 0.73.x  |    79           | October 15, 2019   |              
| 0.74.x  |    80           | November 5, 2019   |              
| 0.75.x  |    80           | December 3, 2019   |              

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

Example Slack ping:

```
@channel 

Versions have migrated to new channels.

- The 0.55.x branch no longer maintained
- 0.56.x branch is now considered Release channel (It is not yet released though, next build will be RC1)
- 0.57.x branch is now considered Beta channel
- 0.58.x branch is now considered Dev channel
- master branch is now considered Nightly channel and is at version 0.59.x.

As usual current version and channel information can be found here:
https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule#current-channel-information

Branch migration tasks that were performed are documented here:
https://github.com/brave/brave-browser/issues/1953
```

