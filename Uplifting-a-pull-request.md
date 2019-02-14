Painfully hand-created table of contents:
- [What are uplifts?](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#what-are-uplifts)
- [Using automation to create those pull requests](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#using-automation-to-create-those-pull-requests)
    - [Getting started](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#getting-started) 
    - [General overview of what happens](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#general-overview-of-what-happens)
    - [Practical examples](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#practical-examples)
    - [Advanced command line usage](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#advanced-command-line-usage)
- [Getting help](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#getting-help)
    - [Command line help](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#command-line-help)
    - [Helpful notes](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#helpful-notes)
    - [Implementation notes](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#implementation-notes)
- [Also see](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request#also-see)

## What are uplifts?
- Most of our pull requests go against master ("nightly")
- If approved by the reviewers, the owner then typically submits the same pull request against the other branches ("dev", "beta", "release") as needed
- The [uplift approvers](https://github.com/brave/brave-browser/wiki/Triage-Guidelines#uplift-approvers) will take a look at those pull requests and approve/deny
- "dev" channel pull requests can be merged without uplift approvers 

## Using automation to create those pull requests
### Getting started
There's a script available in brave-core which helps automate the tedious task of creating all the pull requests:
`./script/pr.py`

In order to use this, you'll need to create a GitHub personal access token
- visit https://github.com/settings/tokens
- click `Generate new token`
- set the scopes (if needed). If you're not sure what to do, you can always change them later
- capture the token that is output
- open your .npmrc (`~/.npmrc`) and add the token value (NOTE: if you do this and store your dotfiles publicly (on GitHub/GitLab/etc), you'll want to specifically add `.npmrc` to your `.gitignore` file)
    ```
    BRAVE_GITHUB_TOKEN=0a0aaa00a0000aa0aaa0000000a0a0000a00aa0a
    ```

If you don't want to store this in your `.npmrc`, you can always set the environment variable or include the environment variable in the usage:
```
BRAVE_GITHUB_TOKEN=0a0aaa00a0000aa0aaa0000000a0a0000a00aa0a ./script/pr.py --options-go-here
```

### General overview of what happens
- changeset is found by diffing local branch against master
- branches are created locally
- all changes are cherry-picked into applicable branches
- local branches are pushed to remote
- a PR is created via the API for each specified channel
- the PRs are then each opened in your default web browser (BRAVE!)

### Practical examples
Let's go over a few scenarios:
1. You finish work on a feature/bug fix/patch. You know it needs to go to BETA. You can specify `--uplift-to=beta`. A pull request will then be created on GitHub against `master`, `dev`, and `beta`:
```
./script/pr.py --reviewers=bbondy,petemill --uplift-to=beta
```
Notice how you can set the reviewers (as a comma separated list)! These will be assigned to all of the PRs which are created

2. You submitted a PR to `master` and it was approved and merged 🎉. You now need to merge this to RELEASE. You can specify `----uplift-using-pr=` along with `--uplift-to=release`. 
```
./script/pr.py --reviewers=bbondy,kjozwiak,rebron,srirambv --uplift-to=release --uplift-using-pr=1632
```
A PR would then be created against `dev`, `beta`, and `release`.

### Advanced command line usage
There are a log of flags you can include. Here are the ones currently present:

1. Want to verify what'll happen (without actually pushing anything!). *Do a dry run*:
```
./script/pr.py --reviewers=bbondy,petemill --uplift-to=beta --dry-run
```
This will create the branches needed, but nothing is sent to GitHub (details are READ from GitHub however, so the token is required). What would have been sent should be shown in stdout

2. Want to see the full API output from GitHub API? *Use verbose mode*
```
./script/pr.py --reviewers=bbondy,petemill --uplift-to=beta --verbose
```
This will show the raw responses sent back from GitHub

3. Want to specify the assignees on the PR? *Use `--owners` (comma separated list)*:
```
./script/pr.py --reviewers=cezaraugusto --owners=bsclifton,petemill --uplift-to=release
```

4. You can specify the title of the PR explicitly *Use `--title`*:
```
./script/pr.py --title="Fix that really bad bug"
```

5. You can specify labels to apply to all PRs *Use `--labels` (comma separated list)*:
```
./script/pr.py --reviewers=jumde --labels=ui,bug
```

6. Do you only want to submit a patch against RELEASE and BETA? (not the others). *Use start-from*:
```
./script/pr.py --reviewers=NejcZdovc --uplift-to=release --start-from=beta
```

## Getting help
- Hit up [@bsclifton](https://github.com/bsclifton) (`@clifton` on Slack)
- On Slack, ask in `#brave-core` or `#brave-core-ci`

### Command line help
You can run the script with `--help` to see all the available inputs:
```
[15:49:17] (~/Documents/brave-browser/src/brave)
clifton@iMac-Pro (npm-run-pr3=) $ ./script/pr.py --help
usage: pr.py [-h] [--reviewers REVIEWERS] [--owners OWNERS]
             [--uplift-to UPLIFT_TO] [--uplift-using-pr UPLIFT_USING_PR]
             [--start-from START_FROM] [-v] [-n] [--labels LABELS]
             [--title TITLE]

create PRs for all branches given branch against master

optional arguments:
  -h, --help            show this help message and exit
  --reviewers REVIEWERS
                        comma separated list of GitHub logins to mark as
                        reviewers
  --owners OWNERS       comma seperated list of GitHub logins to mark as
                        assignee
  --uplift-to UPLIFT_TO
                        starting at nightly (master), how far back to uplift
                        the changes
  --uplift-using-pr UPLIFT_USING_PR
                        link to already existing pull request (number) to use
                        as a reference for uplifting
  --start-from START_FROM
                        instead of starting from nightly (default), start from
                        beta/dev/release
  -v, --verbose         prints the output of the GitHub API calls
  -n, --dry-run         don't actually create pull requests; just show a call
                        would be made
  --labels LABELS       comma seperated list of labels to apply to each pull
                        request
  --title TITLE         title to use (instead of inferring one from the first
                        commit)
```

### Helpful notes
- The "version" used by `master` is determined by looking at the `package.json` in the root of the `brave-browser` repo. At the time of this writing, `0.62.0` is master. When the PR is created for master, this assigns the milestone `0.62.x`
- You can get verbose output with `--verbose`
- You can do a "dry run" with `--dry-run`
- Reviewers + Owner (assignee) are optional
    - If provided, they are a comma separated list
    - usernames / logins are validated against GitHub to make sure they're spelled correctly
    - if omitted, the script will determine the owner by looking at the token
- The script will TRY to set the title automatically:
    - It'll use the subject from the first commit
    - You can override the title using `--title`
- You can provide labels you'd like to apply to each pr using `--labels` (comma separated list)

### Implementation notes
- Available in 0.61.x and newer
- Implemented in https://github.com/brave/brave-core/pull/1632
- Bug fixes in https://github.com/brave/brave-core/pull/1655 and https://github.com/brave/brave-core/pull/1662

## Also see
If you enjoyed this document, check out the following docs too! 😎 
- [Triage Guidelines](https://github.com/brave/brave-browser/wiki/Triage-Guidelines)
- [Checks done on Pull Request](https://github.com/brave/brave-browser/wiki/Checks-done-on-Pull-Request)
- [Brave Release Schedule](https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule)