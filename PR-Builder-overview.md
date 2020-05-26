# PR Builder overview

Every PR in [brave-browser](https://github.com/brave/brave-browser) or [brave-core](https://github.com/brave/brave-core) needs to pass a series of automated checks before merging as described below.

## GitHub overview
On each PR, you should see the checks section as below (unless it's a draft PR or has the `CI/skip` label applied).

![GitHub checks section](images/github-checks.png)

`Details` link will take you to the actual check results (Jenkins private, Travis publicly accessible).

## Jenkins overview
We have a private Jenkins server available at https://ci.brave.com (you need VPN and a Jenkins account).

There are two jobs setup under the `ci` tab:
- [brave-browser-build-pr](https://ci.brave.com/view/ci/job/brave-browser-build-pr)
- [brave-core-build-pr](https://ci.brave.com/view/ci/job/brave-core-build-pr)

Each of these is setup in Jenkins as a multibranch pipeline. A scan is done every 5 minutes for new changes and (once detected) the job will automatically be queued up. Forks are ignored. When a new build starts it will cancel the previously running ones, unless it gets aborted for the following reasons:
- PR labeled with `CI/skip`
- PR is in draft

Extra skipping is available per platform using the `CI/skip-android`, `CI/skip-ios`, `CI/skip-linux`, `CI/skip-macos`, `CI/skip-windows` labels. They are recommended just to save time and resources during development, before merge please take them out and re-run build (unless agreed otherwise with reviewer or uplift approvers).

Slack notifications will be sent to PR author based on a map that associates the GitHub user with their corresponding Slack username. To update, copy the value from our password manager, edit, then update in the Jenkins credential store `github-to-slack-username-map` variable.

Using the UI, you can go into either one of these and then view `Branches` and `Pull Requests`. You can see the history of checks by going into the specific PR or associated branch in Jenkins.

![Brave Core PR builder jobs in Jenkins](images/jenkins-jobs.png)

### Helpful Jenkins links

When on a specific build from the build history there are some helpful links:
- `Console Output` - view full build output
- `Parameters` - view parameter values that have been passed to the build (as defined above)
- `Test Result` - view test results (unit and browser tests together)
- `Replay` - replay build (with option to alter pipeline)
- `Pipeline Steps` - best view for seeing the full list of steps and debugging (can view status and output of individual steps)
- `Workspaces` - view files in the build workspaces and nodes allocated to the build

## Process overview
The checks that are done are defined in the `Jenkinsfile` at the root of the project https://github.com/brave/devops/blob/master/jenkins/jobs/browser/Jenkinsfile.

The above gets called independently by both https://github.com/brave/brave-browser/blob/master/Jenkinsfile and https://github.com/brave/brave-core/blob/master/Jenkinsfile. After the build is done, it will looks for a PR in the other repo and update its status.

We use ephemeral nodes in AWS for building Android, Linux and Windows x64 (which get shutdown if idle for 30m (if no other builds start on them). For macOS we use physical machines (which means higher chance to re-use workspaces).

This `Jenkinsfile` defines the pipeline that builds in parallel for Android `x86`, iOS `arm64`, Linux, macOS and Windowx `x64` with the steps below:
- notify the PR author on Slack that build has started
- checkout source code
- pin locally branch in `package.json` if branch also exists in `brave-core`
- install dependencies (`npm install --no-optional`) and remove `gclient` lock files
- test scripts (`npm run test:scripts`)
- initialize the repository (across runs we do `rm -rf src/brave` to force fetching the latest code then `npm run init`)
- run lint (`npm run lint`)
- audit dependencies (`npm run audit_deps`)
- enable `sccache`
- build
- audit network (`npm run network-audit`)
- unit tests and browser tests (`npm run test -- brave_unit_tests` and `npm run test -- brave_browser_tests`)
- create distributables (and optionally sign)
- upload build artifacts to S3 (`.apk`, `.zip`, `.dmg`, `.pkg`, `.deb`, `.rpm`, `.exe`)
- report build results and link to artifacts via Slack (to PR author and #build-downloads-bot)

To navigate from the `brave-browser-build-pr` or the `brave-core-build-pr` pipeline please go to `Console Output` and press the link to `pr-...`. This will take you to where actually everything gets executed.

Besides the checks done by our Jenkins job, there are some additional checks done via Travis:
- JavaScript lint and unit tests
- security checks
- Python lint (pep8)

## Advanced steps

### Start a PR builder Jenkins job

To build a PR on demand press on the `Build with Parameters` link from the Jenkins job view (`brave-browser-build-pr` or `brave-core-build-pr`). The following parameters are available:
- CHANNEL - `nightly` by default but can be `dev`, `beta` or `release` as well
- BUILD_TYPE - `Release` by default but can be `Debug` as well
- WIPE_WORKSPACE - `false` by default
- SKIP_INIT - `false` by default
- DISABLE_SCCACHE - `false` by default (available only for Android, Linux and macOS)
- SKIP_SIGNING - `true` by default

Same is valid for restarts, always do them from the top level jobs for proper status reporting.

## Resources
- for employees, join the `#brave-browser-ci` Slack channel
- for external contributors (community), we would like to have the content of these checks be publicly viewable in the future
- additional non-public information is available in the [devops wiki](https://github.com/brave/devops/wiki/PR-Builder-Non-public-information)
