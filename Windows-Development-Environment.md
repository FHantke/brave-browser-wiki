# System Requirements

Before you begin, make sure your system satisfies the [system requirements](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#system-requirements).

# Set up Windows

## Visual Studio

Install [Visual Studio Community 2019](https://visualstudio.microsoft.com/vs/older-downloads/).
Follow guidance from the ["Visual Studio" section](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#visual-studio) of the Chromium Windows build instructions.

## Windows 10 SDK

[Install Windows 10 SDK 10.0.20348.0](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive/). (VS 2019 installer does not include this version).

See also https://github.com/google/omaha/blob/main/doc/DeveloperSetupGuide.md#currently-the-supported-toolchain-is-visual-studio-2019-update-161110-and-windows-sdk-100220000

## Git

Install Git from https://git-scm.com/downloads.

Configure Git according to the ["Get the Code" section](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#get-the-code) of the Chromium Windows build instructions, specifically all of the `git config --global` commands.

## Node

Install the Node.js active LTS (v16+) from https://nodejs.org/.

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