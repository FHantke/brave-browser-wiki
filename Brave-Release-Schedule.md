# Overview 

- Major Release frequency: ~3 weeks
- Minor / Chemspill frequency: As needed 
- Every 2nd release is tied to a fuzzy [Chromium release date](https://www.chromium.org/developers/calendar).


# Current channel information:

| **Channel**     | Release |  Beta  | Dev       | Nightly|
| ----------------| ------- | ------ | --------- | ------ |
| **Milestone**   | 0.56.x  | 0.57.x | 0.58.x    | 0.59.x |
| **Branch name** | 0.56.x  | 0.57.x | 0.58.x    | master |


# Freeze

Freeze time is defined to be a period of time where only bug fixes, Chromium minor upgrades, security issues, and privacy issues are accepted. Nothing that smells like a feature can be uplifted in these periods of time.  Text changes are also not accepted unless it's a very bad typo that we missed.  This is important for stability reasons, localization, and making sure there's time to QA and dogfood changes.

If a feature being worked on just misses the freeze time, then it will land on the next branch which is 1 release cycle higher.  This happens even if the feature was originally wanted for a certain version.

Freezing is a concept that is per version branch and not per channel.  Every version has a 3 week freeze before release date.
---

# Release channel dates:

| Version | Chromium | Freeze Date        | Release Date        | Comments                       |
| ------- | ---------|--------------------|---------------------|--------------------------------|
| 0.55.x  |    70    | October 16, 2018   | October 18, 2018    |                                |
| 0.56.x  |    70    | October 30, 2018   | November 6, 2018    |                                |
| 0.57.x  |    71    | November 20, 2018  | December 4, 2018    |                                |
| 0.58.x  |    71    | December 4, 2018   | December 20, 2018   | Early release due to holidays  |
| 0.59.x  |    72    |                    | January 29, 2019    |                                |
| 0.60.x  |    72    |                    | February 19, 2019   |                                |
| 0.61.x  |    73    |                    | Mar 12th, 2019      |                                |
| 0.62.x  |    73    |                    | April 2, 2019       |                                |
| 0.63.x  |    74    |                    | April 23, 2019      |                                |
| 0.64.x  |    74    |                    | May 14, 2019        |                                |
| 0.65.x  |    75    |                    | June 4th, 2019      |                                |
| 0.66.x  |    75    |                    | July 2, 2019        |                                |
| 0.67.x  |    76    |                    | July 30th, 2019     |                                |
| 0.68.x  |    76    |                    | August 20, 2019     |                                |
| 0.69.x  |    77    |                    | Sept 10th, 2019     |                                |
| 0.70.x  |    77    |                    | October 1, 2019     |                                |
| 0.71.x  |    78    |                    | October 22, 2019    |                                |
| 0.72.x  |    78    |                    | November 12, 2019   |                                |
| 0.73.x  |    79    |                    | December 10, 2019   |                                |


- CI will do builds for release channel once a day if there are changes or as requested.
- Updates will be released as requested only.
- Branches here never get re-created, they were created when they were spawned for Dev channel.
- Actual builds are NOT produced here yet even if the date is displayed above until they pass security review.

---

# Beta channel dates:

| Version | Chromium | Date               | Comments                                  |
| ------- | ---------|--------------------|-------------------------------------------|
| 0.52.x  |    68    | July 24, 2018      | Will not ship on Beta channel
| 0.53.x  |    69    | August 14, 2018    | Will not ship on Beta channel
| 0.54.x  |    69    | September 4, 2018  | Will not ship on Beta channel
| 0.55.x  |    70    | September 24, 2018 | Will ship on Beta channel
| 0.56.x  |    70    | October 16, 2018   |
| 0.57.x  |    71    | November 6, 2018   |
| 0.58.x  |    71    | December 4, 2018   |
| 0.59.x  |    72    | December 20, 2018  |
| 0.60.x  |    72    |                    |                     
| 0.61.x  |    73    |                    |                     
| 0.62.x  |    73    |                    |              


 
- CI will eventually do builds for beta channel once a day if there are changes or as requested.
- Updates will be released twice a week or as requested.
- Branches here never get re-created, they were created when they were spawned for Dev channel.
- Actual builds are NOT produced here yet even if the date is displayed above until they pass security review.
- `brave/brave-browser`'s `package.json` specifies brave-core branch version explicitly.

---

# Dev channel dates:

| Version | Target Chromium | Date               | Comments                                  |
| ------- | ----------------|--------------------|-------------------------------------------|
| 0.52.x  |    68           | July 3, 2018       |
| 0.53.x  |    69           | July 24, 2018      |
| 0.54.x  |    69           | August 14, 2018    |
| 0.55.x  |    70           | September 4, 2018  | 
| 0.56.x  |    70           | September 24, 2018 |
| 0.57.x  |    71           | October 16, 2018   |
| 0.58.x  |    71           | November 6, 2018   |
| 0.59.x  |    72           | December 4, 2018   |
| 0.60.x  |    72           | January 15, 2019   |
| 0.61.x  |    73           |                    |                     
| 0.62.x  |    73           |                    |              


- This channel will sometimes get merges from master to speed up the delivery of features. 
- Chromium versions are targets and may not be ready in time for the dates.  If a Chromium upgrade is not ready in time for a branch date, the branch still happens,  but the Chromium upgrade itself will be uplifted onto the Beta channel.
- CI will eventually do builds for Dev channel once a day if there are changes or as requested.
- Updates will be released twice a week or as requested.
- `x` in the version above will increase for each build.
- Branches are created from master on the dates specified.
- `brave/brave-browser`'s `package.json` specifies brave-core branch version explicitly.

---

# Nightly channel:

- CI will eventually do builds for Dev channel once a day if there are changes.
- No updates will be made for Nightly builds.
- See the chart at the top for the version number for Nightly builds.
- With a version such as 0.60.x, the `x` is equal to 0 when dev-channel gets the previous 0.59.y version.
- Developers work off of master.
- We keep master stable, when it is not we backout what broke it.
- `brave/brave-browser`'s `package.json` specifies brave-core branch to be `master`.


# How to do branch migrations:

Follow this checklist:

- [ ] Create a new branch off of master of brave-core for the new version. `cd src/brave && git checkout master && git pull`
- [ ] Create a new branch off of master of brave-browser for the new version.  `git checkout master && git pull`
- [ ] Update info in the chart at the top of https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule 
- [ ] Turn on branch protections for the new branch for brave-browser
- [ ] Turn on branch protections for the new branch for brave-core
- [ ] Update versions on brave-browser with a commit like this: https://github.com/brave/brave-browser/commit/b6e3f38df2519d86c983e12e04a7c0ed70607453
- [ ] Update versions on brave-core with a commit like this: https://github.com/brave/brave-core/commit/dbba50b4bc272ac007522d1e39706594bf78522a
- [ ] Update the `merged/` labels on brave-browser to add the correct prefix for the different versions.
- [ ] Update the `merged/` labels on brave-core to add the correct prefix for the different versions.
- [ ] Update the `uplift/` labels on brave-browser to add the correct prefix for the different versions.
- [ ] Update the `uplift/` labels on brave-core to add the correct prefix for the different versions.
- [ ] Update milestone names with the channel name.
- [ ] Message everyone in slack in #brave-core about a each branch and where it lives.

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

