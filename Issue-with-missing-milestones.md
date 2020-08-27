# Issue with missing milestones

Each valid closed issue should specify a milestone for the version it landed in.

## What is a valid closed issue?

A valid issue is an issue that is closed and does not have one of these labels: `closed/invalid`, `closed/not-actionable`, `closed/fixed-by-component-update`, `closed/works-for-me`, `closed/no-milestone`, `closed/duplicate`,
`closed/wontfix`, `question`, `support`, `closed/stale`

## My issue should not be considered as part of a release, what do I do?

Set one of the above labels.

## How to determine which milestone

The milestone should match the smallest version it landed in.
If you land a PR in `master` and `master` is at `1.12.x`, then the issue should get the `1.12.x` milestone.
If you later uplifted that to `1.11.x` you'd update the issue milestone to `1.11.x` at that time.