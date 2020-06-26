# Labeling

Each valid issue in a release should have exactly one of the following 2 labels: `release-notes/include` or `release-notes/exclude`.  Unsurprisingly, you should use `release-notes/include` if you would like your issue to show up in release notes.

# What should be included in release notes

Pretty much everything except for:

- Dev concerns like refactoring
- Issues about test cases that were added
- Issues that are already covered in another issue
- Regressions introduced inside a milestone which were never seen by release channel

# Proper titles

Something close tot he issue title will be used in release notes. You can see release notes here:
https://github.com/brave/brave-browser/blob/master/CHANGELOG_DESKTOP.md

The issue title do not need to contain the prefix word like "Added", "Updated", "Fixed".