For background, skim the official documentation for [Checking out and Building Chromium on Windows](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md).

# System Requirements

Before you begin, make sure your system satisfies the [system requirements](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#system-requirements) for building Chromium on Windows.

# Installing Windows

*If you already have a Windows installation, skip this section.* 

Windows installation media is available from [Microsoft Online](https://my.visualstudio.com/Downloads?q=Windows%2010). Credentials are stored in the company 1Password under "MSDN bizspark". Once you're logged in, you want to download the third entry in the list: "Windows 10 (multi-edition), Version 1709 (Updated Dec 2017)". Click **Get Key** to obtain activation keys, which you will be asked to provide during installation.

Use the .iso and the activation keys to install Windows in a virtual machine or on hardware (your choice). If you will be virtualizing Windows on macOS, VMWare Fusion is recommended over VirtualBox because it is significantly more stable.

# Setting up Windows

## Visual Studio

Install Visual Studio Community 2017 from https://www.visualstudio.com/downloads/.
Follow guidance from the ["Visual Studio" section](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#visual-studio) of the Chromium Windows build instructions.

## Git

Install Git from https://git-scm.com/downloads.

Configure Git according to the ["Get the Code" section](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#get-the-code) of the Chromium Windows build instructions, specifically all of the `git config --global` commands.

### Working on private repositories

If you will be cloning a private GitHub repository to your new Windows installation, you will need to set up authentication for GitHub. The Git installer includes `ssh` and `ssh-keygen`, so if you use [SSH keys for authentication](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) you can set them up now.

## Node

Install Node LTS from https://nodejs.org/.

## Yarn

Install Yarn from https://yarnpkg.com/lang/en/docs/install/.
Installing Yarn via .msi has been tested and is known to work.

## Python 2.7

Install the latest Python 2.7 release from https://www.python.org/downloads/windows/.

# Done!

Now you are ready to follow the rest of the instructions in the [[wiki|Home]].

## Tip: clone the Brave repository to `C:\`

We recommend cloning the Git repository to the top level of your `C:\` drive because:

- Some developers have encountered problems with paths exceeding 256 chars when the repository is cloned to a subdirectory.
- Some developers have encountered problems with paths that contain spaces (e.g. the path to your home directory, if your username contains spaces) and some of Chromium's build tools.