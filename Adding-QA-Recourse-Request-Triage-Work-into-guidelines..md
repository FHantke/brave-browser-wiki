# General guidelines

- All work has an issue.  Even if it's a small pull request, link it to an issue.
- If you'd like to have an issue schedule to a particular release, use Asana.
- If you'd like to set priority on an issue, use a priority label described below.

# Milestones

## Milestones and releases

- Every time a release happens for something on the Release channel, there is a sign-off on Slack by QA in the #release channel.
- There is 1 milestone per release, but as noted below, a milestone is often based on another milestone in a chain like fashion.  Only one milestone per branch is opened at any given time.

## The QA process around milestones

- There is no difference between an Android milestone and a Desktop milestone. There should just be for example a 1.13.x milestone.
- If there is a second release within a particular branch (e.g. the 1.13.x branch), then we number it 1.13.x Release #2.  If there's a third, 1.13.x Release #3.
- Each milestone within a particular branch, is based on the one before it.  The description of the milestone should link to the milestone before it.
- The description of these milestones will be updated as things are released for them.  QA will make sure that the whole chain of milestones -are tested for the release on the platform that's happening.
- As an example, we can have a `1.13.x Release #2` as the title.  And the description could even be:  
  ```
  Based on 1.13.x Release <link>
  Desktop Hotfix 2
  Android Release 1 
  ```

## Milestones and pull requests

- Pull requests should go into milestones matching the branch name for where they are landing. For master see [src/brave/package.json](https://github.com/brave/brave-core/blob/master/package.json) for the milestone version.

## Milestones and issues

- Issues only get assigned to a milestone once they are completed and merged into that milestone.
- Milestones generally do not track open issues.
  - If an issue should be locked into a particular milestone, and it is not implemented yet, then put a `release/blocking` label on it. This should be rare though.  E.g. a security critical bug, a big regression, or a Chromium upgrade.
  - Please do not change milestones / priority / boards on issues labeled `security` without consulting the security team in Slack (`#security` or `#security-discussion`).

# Projects

- Project boards track areas of work, they should be 1:1 with Pods. 
- A list of all project boards can be found here: https://github.com/brave/brave-browser/projects
- Project boards are a great way to organize priorities (via labels) for a given functional area of work.

## Triaging project boards

- We have weekly triage meetings which includes the following schedule:
    - Go through the projects listed here https://github.com/brave/brave-browser/projects
      - Get a status on each issue
      - Move issues along the process from the left most column to the right most depending on status.
      - Make sure the Untriaged Backlog column is empty and we add appropriate `priority/p1` - `priority/p5` labels
    - Some Projects may have a special triage meeting only for that project.
    - Projects will be added and removed over time.

# Uplifting

- By default everything is PR'ed against master and only lands in master.
- If something is wanted in Dev, Beta, or Release channel, then a PR must be made to the current [version branch](https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule#current-channel-information) and it must get an approval from someone on the [uplift approvers list](https://github.com/brave/brave-browser/wiki/Triage-Guidelines#uplift-approvers) below. That PR must contain the issue link, one PR per channel.
- Milestone on the issue should match only the smallest version where it is landed currently and NOT where you want it ideally.
- Milestones on the PRs should match only the version branch that the PR is being merged to.
- There is a handy script you can use for uplifting! [Check out the docs here](https://github.com/brave/brave-browser/wiki/Uplifting-a-pull-request)
- The person doing the original request must see that the PR actually gets merged.
- This person must only merge, after they see their change in Nightly and test that it works.

# Uplift approvers

- Current approvers are @rebron @kjozwiak @Sri @clifton (Slack usernames).
- There is a [formal GitHub team for the approvers](https://github.com/orgs/brave/teams/uplift-approvers)
- If you are not an approver, do NOT approve any requests made to a Beta or Release branch
- Approvers should be working to keep PRs to version branches at 0 issues. 

# Labels

## Priority labels

We use priority labels from 1-5 as a way to describe which issues should be worked on next. The general principle is:
- if there's a P1 issue you could work on, you should do that at your earliest possible convenience
- don't start work on a P(n+1) issue if there's a P(n) issue you could start working on instead

The rest of this is a rough rubric about how to prioritize issues.

- [`priority/P1`](https://github.com/brave/brave-browser/labels/priority%2FP1): A very extremely bad problem. We might push a hotfix for it. [This should only be for a very few rare issues.]
- [`priority/P2`](https://github.com/brave/brave-browser/labels/priority%2FP2): A bad problem. We might uplift this to the next planned release. [We don't expect to see many of these.]
- [`priority/P3`](https://github.com/brave/brave-browser/labels/priority%2FP3): The next thing for us to work on. It'll ride the trains. [Our bread and butter work.]
- [`priority/P4`](https://github.com/brave/brave-browser/labels/priority%2FP4): Planned work. We expect to get to it "soon". [A larger backlog of things we're getting to.]
- [`priority/P5`](https://github.com/brave/brave-browser/labels/priority%2FP5): Not scheduled. Don't anticipate work on this any time soon. [Not necessarily closed/wontfix, but not in the work queue at all.]

Ultimately, go by the expectation that folks will pick up issues in priority order. If you're noting a task to add to your team's backlog: probably P4. If you're noting a new important task, probably P3 unless unusually severe. If you're triaging a suggestion which shouldn't edge out anything we've already planned: P5.

## QA triage

- Never remove `QA/Yes` and `QA/No` labels
- Things that should get a `QA/No` label: Things that are internal or tooling related only, things that are impossible for QA to test, meta issues, things that are covered by something else fully with a test plan in the same milestone.
- These labels are expected to be added when a PR is submitted, but it gets missed sometimes.
- Issues are NOT guaranteed to be tested on all platforms and device types.
- If your issue needs to be tested on all device types (e.g. Phone / Tablet) then please specify the label `QA/Test-All-Device-Types` on the issue.
- If your issue needs to be tested on all platforms (OS versions) , then please specify the label: `QA/Test-All-Platforms` on the issue.
- QA should block a release if there are any issues with neither of those tags using a search query similar to this one but with an appropriate milestone defined: `is:issue is:closed milestone:"Releasable builds 0.55.x"  -label:"QA/No" -label:"QA/Yes"`.
- QA should ping people as needed to make sure things have one of those labels.
- Add labels for checked once an issue is checked on an OS.  If a certain OS is not needed then indicate it in a comment and mark it as checked too.
- QA should use a search like this to find things that are not yet checked:
  - Linux: `is:issue is:closed milestone:"Releasable builds 0.55.x" -label:"QA/QA Pass-Linux" -label:"QA/No"`
  - macOS: `is:issue is:closed milestone:"Releasable builds 0.55.x" -label:"QA/QA Pass-macOS" -label:"QA/No"`
  - Windows: `is:issue is:closed milestone:"Releasable builds 0.55.x" -label:"QA/QA Pass-Win64" -label:"QA/No"`

## Release notes

- Every issue should be tagged with either `release-notes/include` or `release-notes/exclude`.
- You can use a search term like this to see all unlabeled issues: `is:issue milestone:"0.56.x - Beta" -label:"release-notes/include" -label:"release-notes/exclude"`
- Releases shouldn't go out without release notes.
- Ask QA to generate output when ready.
- PR team should get a rough draft at the start of the Beta period of what will be included in the Beta release. 
- Release notes should be included in a file on the root of brave-browser named CHANGELOG.md.
- On each release, release notes (or a link to release notes) should also be included in the published release.

# Requesting QA resources

QA Work Priority List - https://github.com/brave/qa-resources/projects/2

- When QA resources are needed, create an issue using https://github.com/brave/qa-resources/issues/new/choose and select the appropriate template
- Change the issues `Project` to `QA Work Priority List` which will add the issue into the `Requested/Untriaged Work` column
- If the issue is important and needs immediate attention, label it a `P1` and let QA know via #testers so we can appropriately prioritize/start needed work
- QA will triage the issue(s) within the `Requested/Untriaged Work` column during our Friday standup and move the issue(s) into the `Upcoming/Triaged Work` column
- QA will move the issue into the `In Progress` column to indicate that work as been started/is in progress.
- Once completed, the issue will be closed and automatically moved into the `Completed` column

**`Note`**: Nothing changes for releases that are already scheduled via https://github.com/brave/brave-browser/wiki/Brave-Release-Schedule. QA will continue creating the issues/items under https://github.com/brave/qa-resources/projects/2 and prioritize as needed. This also includes hotfixes.