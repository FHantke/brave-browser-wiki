# General guidelines

1. All work has an issue.
2. All issues must be assigned a milestone.
3. Issues that should be locked into a particular milestone without consultation to the person putting it there, should get the `release/blocking` label added to it.
4. High level summaries should be maintained on this Roadmap wiki for at least everything up to and including the version which is on master: https://github.com/brave/brave-browser/wiki/Roadmap.
5. We have weekly triage meetings to plan what will be in a particular milestone release.  All development work for a particular release should happen while that version is on master, and not while that version is on Dev channel, Beta channel, or Release channel.  Since we're on a time based schedule, nothing will be waiting very long anyway.



# Uplifting

1. After we get to 1.0, uplifting to Dev channel should be rare, uplifting to Beta channel should be extremely rare, hotfixes to release should be reserved for only chemspill type of situations.
To ensure this, add the [`no:milestone` search option when viewing open issues](https://github.com/brave/brave-browser/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+no%3Amilestone).  `1.x milestone` for things that have not been prioritized for a release yet, but that we could see as possibly being prioritized in the near future.  `2.x milestone` for things that are further out.  Use a particular release milestone otherwise to target that release.
2. Things that are uplifted get a changeset SHA of the merge commit pasted into the pull request for each channel.
3. A single label should be added similar to `merged/0.55.x` where right hand side version is the first version this change appears in.    If something is one version it must be in every version above it all the way back up to master. 



# QA triage

- Never remove `QA/Yes` and `QA/No` labels
- Things that should get a `QA/No` label: Things that are internal or tooling related only, things that are impossible for QA to test, meta issues, things that are covered by something else fully with a test plan in the same milestone.
- These labels are expected to be added when a PR is submitted, but it gets missed sometimes.
- QA should block a release if there are any issues with neither of those tags using a search query similar to this one but with an appropriate milestone defined: `is:issue is:closed milestone:"Releasable builds 0.55.x"  -label:"QA/No" -label:"QA/Yes"`.
- QA should ping people as needed to make sure things have one of those labels.
- Add labels for checked once an issue is checked on an OS.  If a certain OS is not needed then indicate it in a comment and mark it as checked too.
- QA should use a search like this to find things that are not yet checked:
  - Linux: `is:issue is:closed milestone:"Releasable builds 0.55.x" -label:"QA/QA Pass-Linux" -label:"QA/No"`
  - macOS: `is:issue is:closed milestone:"Releasable builds 0.55.x" -label:"QA/QA Pass-macOS" -label:"QA/No"`
  - Windows: `is:issue is:closed milestone:"Releasable builds 0.55.x" -label:"QA/QA Pass-Win64" -label:"QA/No"`