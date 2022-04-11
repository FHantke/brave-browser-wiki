## Overview

Minor updates happen between major releases and don't affect the release schedule outlined via https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule

## Maintenance Release
These would be planned releases where we'll deliver bug fixes.

## Hotfix Release

Intended to provide a critical update. Intended for when P1 issues are found/fixed in Brave.

## Minor Chromium release
When Chromium updates are released we're going to commit to getting a new Brave release out within 24 hours. Only Chromium update will be included in the release



### Process for Minor Chromium release

**Note:** some specifics for this may change; this was the process followed for 1.35.x release 4.

1. Create the milestones in [brave-browser](https://github.com/brave/brave-browser/milestones) and [brave-core](https://github.com/brave/brave-core/milestones). For example `1.35.x - Release 4`
1. Create an issue to track this (ex: https://github.com/brave/brave-browser/issues/21241) and put into the milestone
1. Find the latest Desktop and Android tags. An easy way to find would be to look at [CHANGELOG_DESKTOP.md](https://github.com/brave/brave-browser/blob/master/CHANGELOG_DESKTOP.md) and [CHANGELOG_ANDROID.md](https://github.com/brave/brave-browser/blob/master/CHANGELOG_ANDROID.md). Most of the time the tags are the same 👍 
1. We can create a new branch based on this tag. If the tags are the same, we only need one branch. If they are different we can make two branches. For example, if https://github.com/brave/brave-browser/releases/tag/v1.35.103 is the tag, we can run:
    ```
    cd /your-folder-here/brave-browser/
    git fetch --tags
    git checkout -b 1.35.release4 v1.35.103
    git push -u origin 1.35.release4
    cd src/brave
    git checkout -b 1.35.release4 v1.35.103
    git push -u origin 1.35.release4
    ```
1. Run the `chromium-update-minor` job (https://ci.brave.com/view/wip/job/chromium-update-minor/) and specify the branch (ex: `1.35.release4`). This will create a new branch (ex: `1.35.release4-98.0.4758.109`). If this fails, attempt the Chromium update manually. If the tags are NOT the same for Desktop and Android, we'll have needed to create two branches (ex: `1.35.release4` and `1.35.release4android`) and cherry-pick the Chromium update from `1.35.release4` over to `1.35.release4android`
1. Pull request the branch (`1.35.release4-98.0.4758.109` merging into `1.35.release4`)
1. [Uplift approvers group](https://github.com/orgs/brave/teams/uplift-approvers) will Review / Approve / Merge
1. We can start the build via the `brave-browser-build` job. NOTE: We may need to ask for devops support if we can't provide this branch name in the job parameters
1. Once build completes, we need to merge this branch (`1.35.release4`) into the major version branch (`1.35.x`).