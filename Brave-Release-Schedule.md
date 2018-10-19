# Overview 

- Major Release frequency: ~3 weeks
- Minor / Chemspill frequency: As needed 
- Every 2nd release is tied to a fuzzy [Chromium release date](https://www.chromium.org/developers/calendar).


# Current channel information:

| **Channel**     | Release |  Beta  | Dev       | Nightly|
| ----------------| ------- | ------ | --------- | ------ |
| **Milestone**   | 0.55.x  | 0.56.x | 0.57.x    | 0.58.x |
| **Branch name** | 0.55.x  | 0.56.x | 0.57.x    | master |



---

# Release channel dates:

| Version | Chromium | Release Date       | Freeze Date         | Comments                          |
| ------- | ---------|--------------------|---------------------------------------------------------|
| 0.52.x  |    68    | August 14, 2018    |           -         | Will not ship on Release channel  |
| 0.53.x  |    69    | September 4, 2018  |           -         | Will not ship on Release channel  |
| 0.54.x  |    69    | September 24, 2018 |           -         | Will not ship on Release channel  |
| 0.55.x  |    70    | October 16, 2018   |           -         | Will ship on Release channel      |
| 0.56.x  |    70    | November 6, 2018   |  October 30, 2018   |                                   |
| 0.57.x  |    71    | December 4, 2018   |  November 20,2018   |                                   |                                   
| 0.58.x  |    71    | December 20, 2018  |  Always frozen      |                                   |

- CI will eventually do builds for release channel once a day if there are changes or as requested.
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

- [ ] Update versions on brave-browser with a commit like this: https://github.com/brave/brave-browser/commit/b6e3f38df2519d86c983e12e04a7c0ed70607453
- [ ] Update versions on brave-core with a commit like this: https://github.com/brave/brave-core/commit/dbba50b4bc272ac007522d1e39706594bf78522a
- [ ] Message everyone in slack in #brave-core about a each branch and where it lives.
- [ ] Create a new branch off of master of brave-core for the new version.
- [ ] Create a new branch off of master of brave-browser for the new version.
- [ ] Update info in the chart at the top of https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule 
- [ ] Update the labels on brave-browser to add the correct prefix for the different versions.
- [ ] Update the labels on brave-core to add the correct prefix for the different versions.
- [ ] Update milestone names with the channel name.
- [ ] Update projects board with the channel name.

