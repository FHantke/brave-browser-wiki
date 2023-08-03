# Overview

* The Brave browser is released for Linux, macOS, Windows, Android and iOS
* Most of those platforms have a unified preferred method of installation and official packages alone are sufficient
* This is not the case on Linux, where each distribution may have a different package format and method of installation
* [Official](https://brave.com/linux/) Linux packages cover rpm, deb (and snap) package formats delivered via custom repos (and the snap store)
* [Unofficial](https://brave.com/linux/#unofficial-packages) packages are maintained for [Flatpak](https://flathub.org/apps/com.brave.Browser), [Arch](https://aur.archlinux.org/packages?O=0&SeB=nd&K=brave+binary&outdated=&SB=p&SO=d&PP=50&submit=Go), [Manjaro](https://packages.manjaro.org/?query=brave) and [Solus](https://dev.getsol.us/source/brave/repository/master/)
* This page will contain information relevant to (re)packaging the browser for different platforms/distributions

# Recommendations

* Information about released browser versions can be found at [versions.brave.com](https://versions.brave.com/)
* The above page links to individual endpoints for each platform/channel/architecture that return the current version
  * Automation should rely on these endpoints, rather than polling GitHub
* Unless a given package is meant to be unstable, only "public" releases should be used
  * Internal releases are published only to GitHub and are meant primarily for testing (marked as pre-release)
  * Public releases are published to all mediums and are meant for general use (marked as full release)

# Contact

* If you maintain a Brave browser package that we're not aware of, please contact us at devops at brave dot com