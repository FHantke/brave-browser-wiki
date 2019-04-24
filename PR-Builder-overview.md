## Table of Contents
<!-- TOC -->

- [Table of Contents](#table-of-contents)
- [Jenkins overview](#jenkins-overview)
- [GitHub overview](#github-overview)
- [brave-browser checks](#brave-browser-checks)
- [brave-core checks](#brave-core-checks)
- [Questions?](#questions)

<!-- /TOC -->

When a pull request is created in [brave-browser](https://github.com/brave/brave-browser) or [brave-core](https://github.com/brave/brave-core), a series of checks are done. The intention of this page is to describe those checks.

## Jenkins overview
We have a private Jenkins server available at https://staging.ci.brave.com (behind VPN).

There are two jobs setup under the `ci` tab:
- brave-browser-build-pr
- brave-core-build-pr

Each of these is setup in Jenkins as a multibranch pipeline. A scan is done every 5 minutes for new changes and (once detected) the job will automatically be queued up.

Using the UI, you can go into either one of these and then view `Branches` and `Pull Requests`. You can see the history of checks by going into the specific pull request or associated branch in Jenkins.

![Brave Core PR builder jobs in Jenkins](https://media.clifton.io/brave/wiki/jenkins-jobs.png)

## GitHub overview
On each pull request, you should see the checks section
![GitHub checks section](http://media.clifton.io/brave/wiki/github-checks.png)

You can click the `Details` link for the individual check and (for Jenkins checks) it'll launch into the Jenkins interface so you can see the console output.  There are also checks which use https://travis-ci.org which should be publicly accessible.

## brave-browser checks
- Original work done with https://github.com/brave/brave-browser/pull/2226
- Windows is in progress with https://github.com/brave/brave-browser/pull/2985

The checks that are done are defined in the `Jenkinsfile` at the root of the project:
https://github.com/brave/brave-browser/blob/master/Jenkinsfile

This logic currently only runs on Linux and macOS (Windows is WIP) and runs the following:
- initialize the repository (`npm install`, then `npm run init` if needed and finally `npm run sync --all`)
- run lint
- run an official build
- security checks (`npm run test-security`)
- unit tests and browser tests
- keeps build artifacts (`.dmg` file, `.deb` file, `.rpm` file, `.exe` files)

_**Note to reviewers**_: All checks should be passing before you merge a pull request.

## brave-core checks
- Original work done with https://github.com/brave/brave-core/pull/1172

The checks that are done are the same as done with `brave-browser`. There is a `Jenkinsfile` at the root of the project:
https://github.com/brave/brave-core/blob/master/Jenkinsfile

This `Jenkinsfile` does the following:
- Creates a new branch in `brave-browser`
- Updates the `brave-browser` package.json to have the commit hash from the `brave-core` branch
- Creates an instance of the `brave-browser` check

Besides the checks done by our Jenkins job, there are some additional checks done via Travis:
- JavaScript lint / unit tests
- security checks
- Python lint (pep8)

_**Note to reviewers**_: All checks should be passing before you merge a pull request.

## Questions?
- For employees, join the `#brave-core-ci` Slack channel and we'll be happy to answer questions.
    - Additional non-public information is available in the [devops wiki](https://github.com/brave/devops/wiki/PR-Builder-Non-public-information)
- Non-employees (community!), we would like to have the content of these checks be publicly viewable, but there aren't any plans to address that at the moment.
