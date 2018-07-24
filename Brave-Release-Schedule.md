# Brave Release Schedule


## Overview 

|          | Release |  Beta  | Dev | Nightly|
| ---------| ------- | ------ | --------- | ------ |
| **Milestone** | 0.51.x| 0.52.x | 0.53.x | 0.54.x |
| **Branch** | 0.51.x | 0.52.x | 0.53.x | master |


- Major Release frequency: ~3 weeks
- Minor / Chemspill frequency: As needed 
- Every 2nd release is tied to a fuzzy [Chromium release date](https://www.chromium.org/developers/calendar).

---

## Release channel migration dates:

| Version | Chromium | Date               |
| ------- | ---------|--------------------|
| 0.52.x  |    68    | August 14, 2018    |
| 0.53.x  |    69    | September 4, 2018  |
| 0.54.x  |    69    | September 24, 2018 |
| 0.55.x  |    70    | October 16, 2018   |
| 0.56.x  |    70    | November 6, 2018   |
| 0.57.x  |    71    | December 4, 2018   |
| 0.58.x  |    71    | December 20, 2018  |

- CI will eventually do builds for release channel once a day if there are changes.
- Updates will be released as requested only.
- Branches here never get re-created, they were created when they were spawned for Dev channel.

## Beta channel migration dates:

| Version | Chromium | Date               |
| ------- | ---------|--------------------|
| 0.52.x  |    68    | July 24, 2018      |
| 0.53.x  |    69    | August 14, 2018    |
| 0.54.x  |    69    | September 4, 2018  |
| 0.55.x  |    70    | September 24, 2018 |
| 0.56.x  |    70    | October 16, 2018   |
| 0.57.x  |    71    | November 6, 2018   |
| 0.58.x  |    71    | December 4, 2018   |
| 0.59.x  |    72    | December 20, 2018  |
 
- CI will eventually do builds for beta channel once a day if there are changes.
- Updates will be released twice a week or as requested.
- Branches here never get re-created, they were created when they were spawned for Dev channel.

---

## Dev channel migration dates:

| Version | Target Chromium | Date               |
| ------- | ----------------|--------------------|
| 0.52.x  |    68           | July 3, 2018       |
| 0.53.x  |    69           | July 24, 2018      |
| 0.54.x  |    69           | August 14, 2018    |
| 0.55.x  |    70           | September 4, 2018  |
| 0.56.x  |    70           | September 24, 2018 |
| 0.57.x  |    71           | October 16, 2018   |
| 0.58.x  |    71           | November 6, 2018   |
| 0.59.x  |    72           | December 4, 2018   |
| 0.60.x  |    72           | January 15, 2019   |
 
- Chromium versions are targets and may not be ready in time for the dates.  If a Chromium upgrade is not ready in time for a branch date, the branch still happens,  but the Chromium upgrade itself will be uplifted onto the Beta channel.
- CI will eventually do builds for Dev channel once a day if there are changes.
- Updates will be released twice a week or as requested.
- `x` in the version above will increase for each build.
- Branches are created from master on the dates specified.

## Nightly channel:

- CI will eventually do builds for Dev channel once a day if there are changes.
- No updates will be made for Nightly builds.
- See the chart at the top for the version number for Nightly builds.
- With a version such as 0.60.x, the `x` is equal to 0 when dev-channel gets the previous 0.59.y version.
- Developers work off of master.
- We keep master stable, when it is not we backout what broke it.