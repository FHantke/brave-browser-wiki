# PR Builder overview

Every PR in [brave-core](https://github.com/brave/brave-core) needs to pass a series of automated checks before merging as described below. A list of recently built PRs is at https://ci.brave.com/view/pr/

## GitHub overview
On each PR, you should see the checks section as below (unless it's a draft PR or has any `CI/skip` label applied).

![GitHub checks section](images/github-checks.png)

### Helpful Jenkins links

When on a specific build from the build history there are some helpful links:
- `Console Output` - view full build output
- `Parameters` - view parameter values that have been passed to the build (as defined above)
- `Test Result` - view test results (unit and browser tests together)
- `Replay` - replay build (with option to alter pipeline)
- `Pipeline Steps` - best view for seeing the full list of steps and debugging (can view status and output of individual steps)
- `Workspaces` - view files in the build workspaces and nodes allocated to the build

### Start a PR builder Jenkins job

To build a PR on demand press on the `Build with Parameters` link from the Jenkins job view (`brave-core-build-pr`) or https://ci.brave.com/job/brave-core-build-pr-{PLATFORM}/job/PR-{NUMBER}/ where NUMBER is your PR's number and platform is like `android`, `ios`, `linux-x64`, `linux-arm64`, `macos-x64`, `macos-arm64`, `windows-x64`, `windows-arm64`, `windows-x86`. The following parameters are available:
- CHANNEL - `nightly` by default but can be `beta` or `release` as well
- BUILD_TYPE - `Static` by default but can be `Release` as well
- WIPE_WORKSPACE - `false` by default
- USE_RBE - `true` by default
- SKIP_SIGNING - `true` by default
- DCHECK_ALWAYS_ON - `true` by default
- NODE_LABEL - empty by default (auto-determined) - machine where to run the build
- SLACK_NOTIFY - empty by default (auto-set to PR author) - comma-separated list of Slack destinations to notify about build (`@user,#build-bot` for example)

## Jenkins overview
We have a private Jenkins server available at https://ci.brave.com (you need VPN and a Jenkins account).

Each of these is setup in Jenkins as a multibranch pipeline. A scan is done every 5 minutes for new changes and (once detected) the job will automatically be queued up. Forks are ignored. When a new build starts it will cancel the previously running ones, unless it gets aborted for the following reasons:
- PR labeled with `CI/skip` (or platform specific skip labels)
- PR is in draft

Extra skipping is available per platform using the `CI/skip-android`, `CI/skip-ios`, `CI/skip-linux-x64`, `CI/skip-linux-arm64`, `CI/skip-macos-x64`, `CI/skip-macos-arm64`, `CI/skip-windows-x64`, `CI/skip-windows-arm64`, `CI/skip-windows-x86` labels for PRs that do not need to run checks on all platforms.

Slack notifications will be sent to PR author based on a map that associates the GitHub user with their corresponding Slack username. This is generated automatically. 

## Process overview

We use ephemeral nodes in AWS for building Android, Linux and Windows (which get stopped after the build and reused at next one). For iOS and macOS builds we use persistent cloud and physical machines (which means higher chance to re-use workspaces).

This Jenkinsfiles in the devops repo define the steps for building on Android `x86`, iOS `x64`, Linux `x64`, Linux `arm64`, macOS `x64`, macOS `arm64`, Windowx `x64`, Windowx `arm64` and Windowx `x86` with the steps below:
- checkout source code
- install dependencies (`npm install`)
- initialize the repository (across runs we do `npm run sync` to force fetching the latest code or `npm run init` if starting with empty workspace)
- build
- gn check (`npm run gn_check`)
- eslint (`npm run eslint`)
- JavaScript tests
- unit tests and browser tests (`npm run test -- brave_unit_tests` and `npm run test -- brave_browser_tests`)
- upstream tests
- Storybook build
- audit network (`npm run network-audit`)
- create binaries
- upload build artifacts to S3 (`.apk`, `.zip`, `.dmg`, `.pkg`, `.deb`, `.rpm`, `.exe`)
- report build results and link to artifacts via Slack (to PR author/notify list and #build-downloads-bot)

Besides the platform builds, there are pipelines that do platform-agnostic (noplatform) checks like:
- test scripts (`npm run test:scripts`)
- check if rebased against current Chromium version
- audit dependencies (`npm run audit_deps`)

We also use [sonarcloud.io](https://sonarcloud.io) for code quality checks.

More info at https://github.com/brave/devops/wiki/Browser-CI-CD
