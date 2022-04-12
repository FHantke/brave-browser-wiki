Feel free to copy this into an issue if you want to keep track of items per-milestone.
Be super sure that <version> is replaced with the version you wish to use.

### Prerequisites

- [ ] Double check the previous release/milestone and ensure everything has been QA'd
  - **Note:** If there's issues that need to be checked, create a `master` GH issue that lists the issues that need to be QA'd
- [ ] Ensure that the RC can be launched via the GPS internal test channel (basically verifying that `.aab` updates are working)
- [ ] Consult with the security team to ensure that all security issues have been included.
   - [ ] Release using the `critical` feature (forces users to update/restart) if there's a zero day vulnerability being exploited
- [ ] Consult with PR team (@catherinecorre) and provide heads up on release timing, screenshots, other deliverable's.

### Security/Privacy Checks

 - [ ] Download the latest version of [ClassyShark3xodus](https://f-droid.org/en/packages/com.oF2pks.classyshark3xodus/) and ensure that zero trackers are found within Brave.
   - [ ] Once scanned, ensured that selecting `Settings` -> `Sub Totals` also displays zero trackers

**`Example`** | **`Example`**
--------------|---------------
![Screenshot_20220411-220522](https://user-images.githubusercontent.com/2602313/162865301-fe3e4efd-dac0-4403-a8c9-5bee07881341.png) | ![Screenshot_20220411-220913](https://user-images.githubusercontent.com/2602313/162865300-6c4b5929-a5ac-47fe-9df4-4faca1f36534.png)

### Release Notes to Staging

- [ ] Mark closed issues in github as `release-notes/exclude` or `release-notes/include`.
- [ ] Commit release notes to [CHANGELOG_Android.md](https://github.com/brave/brave-browser/blob/master/CHANGELOG_ANDROID.md) in the [`brave-browser`](https://github.com/brave/brave-browser/) `master` branch
- [ ] Stage release notes to https://github.com/brave/brave-browser/releases/