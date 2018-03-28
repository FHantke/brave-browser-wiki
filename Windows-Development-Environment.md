# System Requirements

Before you begin, make sure your system satisfies the [system requirements](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#system-requirements).

# Set up Windows

## Visual Studio

Install Visual Studio Community 2017 from https://www.visualstudio.com/downloads/.
Follow guidance from the ["Visual Studio" section](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#visual-studio) of the Chromium Windows build instructions.

## Git

Install Git from https://git-scm.com/downloads.

Configure Git according to the ["Get the Code" section](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#get-the-code) of the Chromium Windows build instructions, specifically all of the `git config --global` commands.

## Node

Install Node LTS from https://nodejs.org/.

## Yarn

Install Yarn from https://yarnpkg.com/lang/en/docs/install/.
Installing Yarn via .msi has been tested and is known to work.

## Python 2.7

Install the latest Python 2.7 release from https://www.python.org/downloads/windows/.

## Done!

Now you are ready to follow the next step of the build instructions in the [[wiki|Home]].

# Troubleshooting

The upstream documentation for [Checking out and Building Chromium on Windows](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md) have a lot of useful information on configuring Windows, resolving common problems, speeding up builds, etc.

## Clone the Brave repository to `C:\`

We recommend cloning the `brave-browser` Git repository to the top level of your `C:\` drive because:

- Some developers have encountered problems with paths exceeding 256 chars when the repository is cloned to a subdirectory.
- Some developers have encountered problems with paths that contain spaces (e.g. the path to your home directory, if your username contains spaces) and some of Chromium's build tools.