# System Requirements

Before you begin, make sure your system satisfies the [system requirements](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#system-requirements).

# Set up Windows

## Visual Studio

Install the latest version of Visual Studio Community 2017 from https://my.visualstudio.com/Downloads?q=visual%20studio%20community%202017.
Follow guidance from the ["Visual Studio" section](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#visual-studio) of the Chromium Windows build instructions. The 2017 edition is currently preferred because

* Our continuous integration bots run VS2017
* [The newest version of VS2019 can not build Chromium](https://crbug.com/1058860)

## Windows 10 SDK

[Install Windows 10 SDK 10.0.18362.0](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk/). (VS2017 installer does not include this version.)

## Git

Install Git from https://git-scm.com/downloads.

Configure Git according to the ["Get the Code" section](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#get-the-code) of the Chromium Windows build instructions, specifically all of the `git config --global` commands.

## Node

Install Node LTS from https://nodejs.org/.

## npm or Yarn

Preferably use npm as it is the default package manager used across our builds. npm is built in Node by default. Alternatively, you can install Yarn:

Install Yarn from https://yarnpkg.com/lang/en/docs/install/.
Installing Yarn via `.msi` has been tested and is known to work.

If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

## Python 2.7

Install the latest Python 2.7 release from https://www.python.org/downloads/windows/.

## Done!

Now you are ready to follow the next step of the build instructions in the [[wiki|Home]].

# Running Brave

It's always best to run Brave from a standard `cmd.exe` shell or via Windows Explorer. There are instances where debug builds of Brave attempt to log to stderr, which will fail and potentially result in unexpected crashes in non-standard shells (e.g., Cygwin, Git for Windows, etc.)

# Troubleshooting

The upstream documentation for [Checking out and Building Chromium on Windows](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md) have a lot of useful information on configuring Windows, resolving common problems, speeding up builds, etc.

## Clone the Brave repository to `C:\`

We recommend cloning the `brave-browser` Git repository to the top level of your `C:\` drive because:

- Some developers have encountered problems with paths exceeding 256 chars when the repository is cloned to a subdirectory.
- Some developers have encountered problems with paths that contain spaces (e.g. the path to your home directory, if your username contains spaces) and some of Chromium's build tools.