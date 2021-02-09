## Introduction
The iOS version of Brave can be found in the [brave-ios](https://github.com/brave/brave-ios) repo. All development (issues, PRs, etc) takes place there.

The iOS code does use `brave-core` for a few things:
- Brave Rewards
- Brave Ads
- Sync

Changes to those require the developer to compile `brave-core`.

## Getting started
NOTE: at the time of this writing, building on the M1 (arm64) is not possible 😢

### Prerequisites
You'll want to check the official Chromium docs for the latest requirements:
https://chromium.googlesource.com/chromium/src/+/lkgr/docs/mac_build_instructions.md#system-requirements

Besides these, you'll want to have a few other items installed. Check those out here:
https://github.com/brave/brave-browser/wiki/macOS-Development-Environment

### Setting up sccache
Building can take a long time! Luckily, we can use sccache to speed things up. You'll have to request access from Devops and you'll be given credentials and an S3 bucket that you can use. 

You can follow the instructions under [Configuring sccache](https://github.com/brave/brave-browser/wiki/sccache-for-faster-builds#configuring-sccache) > [Shared S3 cache](https://github.com/brave/brave-browser/wiki/sccache-for-faster-builds#shared-s3-cache)


## TODOs
- get jenkins job compiling the iOS bucket (multiple branches? debug/release? how often?)