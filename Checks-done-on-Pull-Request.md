When a pull request is issued for [brave-browser](https://github.com/brave/brave-browser) or [brave-core](https://github.com/brave/brave-core), a series of checks are done. The intention of this page is to describe those checks

## Jenkins overview
We have a non-public Jenkins server setup at https://staging.ci.brave.com/ (access limited to Brave employees - NOTE: VPN is required).

There are two jobs setup under the `ci` tab.
- brave-browser-build-pr
- brave-core-build-pr

Each of these is setup in Jenkins as a GitHub repository. A scan is done every 5 minutes for new changes and (once detected) the job will automatically be queued up. Using the UI, you can go into either one of these and then view `Branches` and `Pull Requests`. *Each time a pull request has a change pushed, it will queue up a new check*. You can see the history of checks by going into the specific pull request in Jenkins

## GitHub overview
On each pull request, you should see the checks section
![GitHub checks section](http://media.clifton.io/brave/wiki/github-checks.png)

You can click the `Details` link for the individual check and (for Jenkins checks) it'll launch into the Jenkins interface so you can see the console output.  There are also checks which use travis-ci.org which should be publicly accessible.

## brave-browser checks
- Original work done with https://github.com/brave/brave-browser/pull/2226
- Windows added with https://github.com/brave/brave-browser/pull/2985

TODO

## brave-core
- Original work done with https://github.com/brave/brave-core/pull/1172

TODO

## Questions?
For employees, join the `#brave-core-ci` channel and we'll be happy to answer questions. 
