# Overview

* The Brave browser is released for Linux, macOS, Windows, Android and iOS
* Most of those platforms have a unified preferred method of installation and official packages alone are sufficient
* This is not the case on Linux, where each distribution may have a different package format and method of installation
* [Official](https://brave.com/linux/) Linux packages cover RPM and DEB package formats delivered via custom repos, as well as the [Flatpak](https://flathub.org/apps/com.brave.Browser), and [Snap](https://snapcraft.io/brave).
* [Unofficial](https://brave.com/linux/#unofficial-packages) packages are maintained for [Arch](https://aur.archlinux.org/packages?O=0&SeB=nd&K=brave+binary&outdated=&SB=p&SO=d&PP=50&submit=Go), [Manjaro](https://packages.manjaro.org/?query=brave), [Solus](https://dev.getsol.us/source/brave/repository/master/) and [F-Droid](https://f-droid.org/en/packages/de.marmaro.krt.ffupdater/)
* This page will contain information relevant to (re)packaging the browser for different platforms/distributions

# Recommendations

* Information about released browser versions can be found at [versions.brave.com](https://versions.brave.com/)
* Unless a given package is meant to be unstable, only "public" releases should be used
  * Internal releases are published only to GitHub and are meant primarily for testing (marked as pre-release)
  * Public releases are published to all mediums and are meant for general use (marked as full release)
* The above page links to individual endpoints for each platform/channel/architecture that return the current version
  * E.g. public linux x64 release - https://versions.brave.com/latest/release-linux-x64.version
  * Automation should rely on these endpoints, rather than polling GitHub

# Contact

* If you maintain a Brave browser package that we're not aware of, please contact us at devops at brave dot com