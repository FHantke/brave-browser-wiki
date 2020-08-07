# Labeling

We use the same repository for both Android and Desktop, and iOS uses this repository as a dependency.
Additionally, we sometimes have code which only applies to one particular operating system. To help QA we ask that each valid issue in a release should have at least one OS label.

The available OS labels are:

- `OS/macOS`
- `OS/Windows`
- `OS/Linux`
- `OS/Android`
- `OS/Desktop`
- `OS/iOS`

If an issue applies to all desktop operating systems, it's preferred to use `OS/Desktop` over the more specific ones. Although if you want to over emphasize that testing is needed on every platform, use the specific ones.

# When to apply a label

If the change in code will affect the platform, then you should specify the label for that platform.
Alternatively, if the changes will affect the builds which are produced, then you should specify those platform labels.
Alternatively, if the change is just internal to developers, such as build scripts, then specify the platforms that will be affected for developers on those platforms. 
Alternatively, if the change affects an extension that we track in brave-browser, it is probably only for desktop since we only have extensions on desktop.

# How does QA use OS labels?

OS labels will be ignored for the purpose of testing by the QA team if the issue has a `QA/No` label.
If the issue has a `QA/Yes` label, then it will be tested on the operating systems listed in the OS labels.
If QA sees a `QA/Desktop` label (instead of the more specific `OS/macOS`, `OS/Windows`, and `OS/Linux`), then the issue may only be tested on one of the desktop platforms.

The QA team may also use these labels in conjunction with the `release-notes/include` label to create release notes.
Desktop and Android are deployed at different times in different product releases, so have different release notes.