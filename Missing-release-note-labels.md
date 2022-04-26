# Labeling

Each valid issue in a release should have exactly one of the following 2 labels: `release-notes/include` or `release-notes/exclude`.  Unsurprisingly, you should use `release-notes/include` to indicate that your issue should appear in release notes.

# What should be _included_ in release notes

No matter how insignificant, if there is a change that was ever possible for any user in Release channel to experience, it should be `release-notes/include`. Don’t select the status of release-notes based on how important you think the change is. Most things worked on are usually `release-notes/include`. This applies equally to new features as it does to UI tweaks and bugfixes.

# What should be _excluded_ from release notes

`release-notes/exclude` is appropriate in a few special cases:

- Purely `dev-concern` issues like refactoring, build configuration changes, or changes to test code
- Issues that are already covered in another issue
- Regressions introduced inside a milestone which were never seen by Release channel
- Internal changes like a Mojo API being added in support of some other issue
- Changes to support an unreleased feature that will be made available to users in a future version (e.g. to be rolled out via Griffin)

# Proper titles

Something close to the issue title will be used in release notes. You can see release notes here:
https://github.com/brave/brave-browser/blob/master/CHANGELOG_DESKTOP.md

The issue title do not need to contain the prefix word like "Added", "Updated", "Fixed".