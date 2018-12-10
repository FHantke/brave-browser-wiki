# General guidelines

- All work has an issue.
- If you'd like to have an issue schedule to a particular release, add it to the closest matching project board for triage.  If you aren't sure use the project called `General`.  A list of all project boards can be found here: https://github.com/brave/brave-browser/projects
- Issues only get assigned to a milestone once they are completed and merged into that milestone.
- Issues that should be locked into a particular milestone without consultation to the person putting it there, should get the `release/blocking` label added to it and can go into a milestone while the issue is still open. This should be rare though.  E.g. a security critical bug, or a Chromium upgrade.
- Please do not change milestones/priority/boards on issues labeled `security` without consulting the security team in Slack (`#security` or `#security-discussion`).
- High level summaries should be maintained on this Roadmap wiki for at least everything up to and including the version which is on master: https://github.com/brave/brave-browser/wiki/Roadmap.
- We have weekly triage meetings which includes the following schedule:
    - Go through the projects listed here https://github.com/brave/brave-browser/projects
      -  Get a status on each issue
      - Move issues along the process from the left most column to the right most depending on status.
      - Make sure the Untriaged Backlog column is empty and we add appropriate `priority/p1` - `priority/p5` labels
    - Go through closed PRs that were merged to master recently.  Flag anything that we'd like to uplift, add an uplift label.  
    - Some Projects may have a special triage meeting only for that project.
    - Projects will be added and removed over time.


# Uplifting

- By default everything only lands in master.
- For now no approval is needed if you also want it in Dev channel.  This is subject to change in the future.
- Milestone on the issue should match only where it is currently and not where you want it ideally.  Merged label on the PR should match only where it is merged up to and not where you want it ideally.
- Beta requires special permissions now. You can simply add a label to the PR of for example: `uplift-request/0.56.x-Beta` and it'll be either approved or not by one of the approvers (See below).  Once merged milestone will be updated accordingly on the issue and merge label will be updated accordingly as well. 
- When something is uplifted, a changeset SHA of the merge commit should be pasted into the pull request for each branch it is uplifted to.
- A single label should be added similar to `merged/0.55.x` where right hand side version is the first version this change appears in.    If something is one version it must be in every version above it all the way back up to master. For example if master is 0.58.x and it is marked with `merged/0.55.x`. Then this would go into 0.55.x, 0.56.x, 0.57.x, and master.

# Uplift approvers

- Current approvers are @rebron @kjozwiak @Sri (Slack usernames).
- They are expected to keep on top of issues tagged with labels similar to: `uplift-request/0.56.x-Beta`.
- They should be working to keep that list at 0 issues. This happens in 1 of 2 ways.
  1. If approved, they will add an `uplift-approved/0.56.x-Beta` label. 
  2. If not approved, they will indicate the decision in the issue with something like "This is too risky so we won't uplift to Beta, you can bring it up at the next triage meeting to dispute it.".  Then they will remove the uplift request label.
- Approvers are expected to work with people across the org to help make decisions.
- The person doing the original request must see that it actually gets merged. This includes updating the PR merged label and the issues milestone.

# Priority labels

We use priority labels from 1-5 as a way to describe which issues should be worked on next. The general principle is:
- if there's a P1 issue you could work on, you should do that at your earliest possible convenience
- don't start work on a P(n+1) issue if there's a P(n) issue you could start working on instead

The rest of this is a rough rubric about how to prioritize issues.

- [`priority/P1`](https://github.com/brave/brave-browser/labels/priority%2FP1): A very extremely bad problem. We might push a hotfix for it. [This should only be for a very few rare issues.]
- [`priority/P2`](https://github.com/brave/brave-browser/labels/priority%2FP2): A bad problem. We might uplift this to the next planned release. [We don't expect to see many of these.]
- [`priority/P3`](https://github.com/brave/brave-browser/labels/priority%2FP3): The next thing for us to work on. It'll ride the trains. [Our bread and butter work.]
- [`priority/P4`](https://github.com/brave/brave-browser/labels/priority%2FP4): Planned work. We expect to get to it "soon". [A larger backlog of things we're getting to.]
- [`priority/P5`](https://github.com/brave/brave-browser/labels/priority%2FP5): Not scheduled. Don't anticipate work on this any time soon. [Not necessarily wontfix, but not in the work queue at all.]

Ultimately, go by the expectation that folks will pick up issues in priority order. If you're noting a task to add to your team's backlog: probably P4. If you're noting a new important task, probably P3 unless unusually severe. If you're triaging a suggestion which shouldn't edge out anything we've already planned: P5.

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


# Release notes

- Every issue should be tagged with either `release-notes/include` or `release-notes/exclude`.
- You can use a search term like this to see all unlabeled issues: `is:issue milestone:"0.56.x - Beta" -label:"release-notes/include" -label:"release-notes/exclude"`
- Releases shouldn't go out without release notes.
- Ask QA to generate output when ready.
- PR team should get a rough draft at the start of the Beta period of what will be included in the Beta release. 
- Release notes should be included in a file on the root of brave-browser named CHANGELOG.md.
- On each release, release notes (or a link to release notes) should also be included in the published release.