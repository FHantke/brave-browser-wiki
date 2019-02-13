## What are uplifts?
- All of our pull requests typically go against master ("nightly")
- If approved, the owner can submit the same pull request against the other branches ("dev", "beta", "release") as needed
- The [uplift approvers](https://github.com/brave/brave-browser/wiki/Triage-Guidelines#uplift-approvers) will take a look at those pull requests and approve/deny

## Using automation to create those pull requests
TODO:

## Example command line usage(s):
NOTE: you can either pass the token in as an environment variable OR put it in your `~/.npmrc`. Either way would work. The NAME of the variable needs to be: `BRAVE_GITHUB_TOKEN`

1. Create 4 PRs, based on your local branch (which would be branched from master). One for master (`nightly`), `dev`, `beta`, and finally `release`. Will set the reviewers as @bbondy and @petemill and the assignee as @bsclifton 
`./script/pr.py --reviewers=bbondy,petemill --owners=bsclifton --uplift-to=release`

2. Same as above, but don't actually submit the PRs (dry run):
`./script/pr.py --reviewers=bbondy,petemill --owners=bsclifton --dry-run --uplift-to=release`

3. Submit 3 PRs. Use case would be that master already has a PR up and now it's time to start the uplift process:
`./script/pr.py --reviewers=bbondy,petemill --owners=bsclifton --uplift-to=release --start-from=dev`

4. Get more verbose output (otherwise, same as number 3 above):
`./script/pr.py --reviewers=bbondy,petemill --owners=bsclifton --uplift-to=release --start-from=dev --verbose`

5. Show all possible parameters (with description):
`./script/pr.py --help`

6. Uplift an already existing pull request (https://github.com/brave/brave-core/pull/1632) to dev.
`./script/pr.py --reviewers=bbondy,petemill,NejcZdovc --owners=bsclifton --uplift-to=dev --labels=ui --dry-run --uplift-using-pr=1632`

## FIX ME
- You finish work on a feature/bug fix/patch. You know it needs to go to BETA. You can use this and specify `--uplift-to=beta`. A PR will then be created against `master`, `dev`, and `beta`.
- You submitted a PR to `master` and it was approved/merged. You now need to merge this to RELEASE. You can specify `----uplift-using-pr=12345` (putting the actual PR number), along with `--uplift-to=release` and `--start-from=dev` (since master was already approved). A PR would then be created against `dev`, `beta`, and `release`.

## Notes
- The "version" used by `master` is determined by looking at the package.json in `brave-browser`. For example, `0.62.0` will map itself to the milestone `0.62.x`
- You can create a personal token via GitHub on your settings page
- You can get verbose output with `--verbose`
- You can do a "dry run" with `--dry-run`
- Reviewers + Owner (assignee) are optional. If provided, they are a comma separated list.
- You can override the title using `--title`
- You can provide labels you'd like to apply to each pr using `--labels` (comma separated list)

## Implementation notes
- Available in 0.61.x and newer
- Implemented in https://github.com/brave/brave-core/pull/1632
- Bug fixes in https://github.com/brave/brave-core/pull/1655 and https://github.com/brave/brave-core/pull/1662