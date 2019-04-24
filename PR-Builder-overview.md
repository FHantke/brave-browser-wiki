## Table of Contents
<!-- TOC -->

[Table of Contents](#table-of-contents)

[GitHub overview](#github-overview)

[Jenkins overview](#jenkins-overview)

[brave-browser checks](#brave-browser-checks)

[brave-core checks](#brave-core-checks)

[Advanced](#advanced)

[Debugging](#debugging)

[Resources](#resources)

<!-- /TOC -->

Every PR in [brave-browser](https://github.com/brave/brave-browser) or [brave-core](https://github.com/brave/brave-core) needs to pass a series of automated checks before merging. The intention of this page is to describe those checks.

## GitHub overview
On each PR, you should see the checks section as below (unless it's a draft PR or has the `CI/Skip` label applied).

![GitHub checks section](github-checks.png)

`Details` link will take you to the actual check results (Jenkins private, Travis publicly accessible).

## Jenkins overview
We have a private Jenkins server available at https://staging.ci.brave.com (you need VPN and a Jenkins account).

There are two jobs setup under the `ci` tab:
- [brave-browser-build-pr](https://staging.ci.brave.com/view/ci/job/brave-browser-build-pr)
- [brave-core-build-pr](https://staging.ci.brave.com/view/ci/job/brave-core-build-pr)

Each of these is setup in Jenkins as a multibranch pipeline. A scan is done every 5 minutes for new changes and (once detected) the job will automatically be queued up. Forks are ignored.

Using the UI, you can go into either one of these and then view `Branches` and `Pull Requests`. You can see the history of checks by going into the specific PR or associated branch in Jenkins.

![Brave Core PR builder jobs in Jenkins](jenkins-jobs.png)

## brave-browser checks
The checks that are done are defined in the `Jenkinsfile` at the root of the project https://github.com/brave/brave-browser/blob/master/Jenkinsfile

This `Jenkinsfile` defines the pipeline that builds in parallel for Linux, macOS and Windowx x64 with the steps below:
- initialize the repository (`npm install --no-optional`, then `npm run init` if needed and finally `npm run sync -- --all`)
- run lint (`npm run lint`)
- build as an official build
- create distributables
- security checks (`npm run test-security`)
- unit tests and browser tests (`npm run test -- brave_unit_tests` and `npm run test -- brave_browser_tests`)
- upload build artifacts to S3 (`.dmg` file, `.deb` file, `.rpm` file, `.exe` files)

We use ephemeral nodes in AWS for building Linux and Windows x64 (which get shutdown if idle for 30m - if no other builds start on them). For macOS we use physical machines (which means higher chance to re-use workspaces).

## brave-core checks
The checks here are executed by calling the `brave-browser` pipeline as defined in https://github.com/brave/brave-core/blob/master/Jenkinsfile

This `Jenkinsfile` defines the pipeline that does:
- create a new branch in `brave-browser` if it doesn't exist
- pin the `brave-core` branch in package.json from `brave-browser`
- if versions from `package.json` are different between the 2 repos then do a rebase on `brave-browser` against PR target branch
- waits for 6m for the new branch to be discovered by the `brave-browser` pipeline
- calls the `brave-browser` pipeline

Besides the checks done by our Jenkins job, there are some additional checks done via Travis:
- JavaScript lint and unit tests
- security checks
- Python lint (pep8)

## Advanced steps

To build a PR on demand press on the `Build with Parameters` link from the Jenkins job view. The following parameters are available:
- BRANCH - `master` by default (ignored if you're building a PR)
- CHANNEL - `dev` by default but can be `nightly`, `beta` or `release` as well
- WIPE_WORKSPACE - `false` by default
- RUN_INIT - `false` by default
- DISABLE_SCCACHE - `false` by default (only on Linux and macOS)
- BUILD_LINUX - `true` by default
- BUILD_MAC - `true` by default
- BUILD_WINDOWS_X64 - `true` by default
- BUILD_WINDOWS_IA32 - `false` by default
- DEBUG - `false` by default

When on a specific build from the build history there are some helpful links:
- `Console Output` - view full build output
- `Parameters` - view parameter values that have been passed to the build (as defined above)
- `Test Result` - view test results (unit and browser tests together)
- `Replay` - replay build (with option to alter pipeline)
- `Pipeline Steps` - best view for seeing the full list of steps and debugging (can view status and output of individual steps)
- `Workspaces` - view files in the build workspaces

## Debugging

## Resources
- for employees, join the `#brave-core-ci` Slack channel
- for external contributors (community), we would like to have the content of these checks be publicly viewable in the future
- additional non-public information is available in the [devops wiki](https://github.com/brave/devops/wiki/PR-Builder-Non-public-information)
