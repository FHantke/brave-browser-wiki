## Overview
Minor updates happen between major releases and don't affect the release schedule outlined via https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule

## Maintenance release
These would be planned releases where we'll deliver bug fixes.

## Hotfix
Intended to provide a critical update

- When Chromium updates are released we're going to commit to getting a new Brave release out within 24 hours.
    - Only Chromium update will be included in the release
- When P1 issues are found/fixed in Brave


### Process for hotfix
1. Create the milestones in [brave-browser](https://github.com/brave/brave-browser/milestones) and [brave-core](https://github.com/brave/brave-core/milestones). For example `1.35.x - Release 4`
1. Create an issue to track this (ex: https://github.com/brave/brave-browser/issues/21241) and put into the milestone
1. Find the latest Desktop and Android tags. An easy way to find would be to look at [CHANGELOG_DESKTOP.md](https://github.com/brave/brave-browser/blob/master/CHANGELOG_DESKTOP.md) and [CHANGELOG_ANDROID.md](https://github.com/brave/brave-browser/blob/master/CHANGELOG_ANDROID.md). Most of the time the tags are the same 👍 
1. If the tags are the same we can create a new branch based on this tag. For example, if https://github.com/brave/brave-browser/releases/tag/v1.35.103 is the tag, we can run:
    ```
    git checkout -b 1.35.release4 v1.35.103
    git push -u origin2 1.35.release4
    ```
