# Setup build environment

There are some documents about how to build/customize [google omaha](https://github.com/google/omaha).
* [Developer Setup Guide](https://github.com/google/omaha/blob/master/doc/DeveloperSetupGuide.md) from google omaha 
* [Tutorial](https://fman.io/blog/google-omaha-tutorial/) and [build step](https://fman.io/blog/creating-your-own-fork-of-google-omaha) from fman.io
* [Wiki pages](https://github.com/Crystalnix/omaha) from crystalnix

I think all of above guides give good advice to start.

Among them, I recommend first one([Developer Setup Guide](https://github.com/google/omaha/blob/master/doc/DeveloperSetupGuide.md)).

If you follow that instruction w/o missing anything, build will be completed.

About Visual Studio 2015 installation, refer to crystalnix's document about [How to install Visual Studio 2015](https://github.com/Crystalnix/omaha/wiki/How-to-install-Visual-Studio-2015) about which components must be included.

About third party, you just need to clone breakpad and googletest to its folder in the ./omaha/third_party.

I installed libraries and tools like below.

| Module | Path |  |
|----------|----------|---|
|     atl     |    C:\atl      | extracted sourceCode.zip in Atlserver.zip to C:\atl |
|     go     |     C:\go     |  |
|    pstools      |     C:\pstools     |  |
|     protobuf     |     C:\protobuf     | In this folder, you will have bin, include and src subfolers |
|     Python24     |   C:\Python24       | Checks there is C:\Python24\Lib\site-packages\scons-1.3.1 |
|    swtoolkit      |     C:\swtoolkit     |  |
| wtl | C:\wtl |  |


When you finish required tools installation , you need to modify `omaha/hammer.bat` to reflect install locations on local machine.

In my case, I changed only below variables. Others are same with upstream configurations.

`set OMAHA_PSEXEC_DIR=C:\pstools`

`set OMAHA_ATL_SERVER_DIR=C:\atl`

`set OMAHA_WTL_DIR=C:\wtl`

If you installed WIX v3.11, you need to modify `OMAHA_WIX_DIR`.
`set OMAHA_WIX_DIR=%ProgramFiles(x86)%\Wix Toolset v3.11\bin`.

Then, let's start by run `hammer` in `omaha` folder that includes `hammer.bat`.

If you see `VisualStudioVersion variable is not set.` `Have you run vcvarsall.bat before running this script?` message, you should execute that script `C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat` before running `hammer`.

Instead of running `vcvarsall.bat` explicitly, you can add `call "%VS140COMNTOOLS%..\..\VC\vcvarsall.bat"` to `hammer.bat`.
Or, use `Developer Command Prompt for VS2015` instead of `Command Prompt`. 

If build stops w/o showing specific error log, just try `hammer` again.

For optimized build, use `hammer MODE=opt-win` instead of `hammer`.

For more hammer options, see [Hammer Options](https://github.com/google/omaha/blob/master/doc/HammerOptions.md).

When build is completed, you can see below log.
```
...
Install file: "scons-out\dbg-win\obj\recovery\repair_exe\custom_action\testelevateusingmsp.exe" as "scons-out\dbg-win\tests\testelevateusingmsp.exe"
Install file: "scons-out\dbg-win\obj\recovery\repair_exe\custom_action\testelevateusingmsp.pdb" as "scons-out\dbg-win\tests\testelevateusingmsp.pdb"
scons: done building targets.

c:\Projects\omaha\omaha>
```

### Troubleshooting guide

#### 1. 'uuidgen.exe' is not recognized

```
python C:\Projects\omaha\omaha\tools\proxy_clsid_utils.py
Traceback (most recent call last):
  File "C:\Projects\omaha\omaha\tools\proxy_clsid_utils.py", line 152, in ?
    _Main()
  File "C:\Projects\omaha\omaha\tools\proxy_clsid_utils.py", line 148, in _Main
    _GenerateProxyClsidsFiles()
  File "C:\Projects\omaha\omaha\tools\proxy_clsid_utils.py", line 113, in _GenerateProxyClsidsFiles
    machine_proxy_clsid = _GenerateGuid()
  File "C:\Projects\omaha\omaha\tools\proxy_clsid_utils.py", line 46, in _GenerateGuid
    raise SystemError("Failed to get GUID: %s" % guid)
SystemError: Failed to get GUID: 'uuidgen.exe' is not recognized as an internal or external command,
operable program or batch file.
```

When you see this error, Visual Studio 2015 might not be installed properly.

You can quickly check by running `uuidgen` in the Developer Command Prompt For VS2015. If it fails, please check about installed components of Visual Studio 2015 and refer again above crystalnix's VS2015 install guide.



#### 2. error:  WTL requires WINVER >= 0x0501

```
C:\wtl\include\atlapp.h(35): fatal error C1189: #error:  WTL requires WINVER >= 0x0501
scons: *** [scons-out\dbg-win\obj\base\base_pch.pch] Error 2
```

When you see this version error, you downloaded latest WTL. Latest WTL(V10) added this version check.
If you use [WTL 9.1](http://wtl.sourceforge.net/), you will not see this error.

To fix this, you need to modify `WINVER` value from `0x0500` to `0x0501` in `omaha/omaha/main.scon`.
```
CPPDEFINES = [
    ...
    'WINVER=0x0501',
    ...
]
```

#### 3. error C2059: syntax error: 'bad suffix on number'
```
________Compiling scons-out\dbg-win\obj\common\update3_utils.obj
update3_utils.cc
C:\Projects\omaha\omaha/goopdate/com_proxy.h(89): error C2059: syntax error: 'bad suffix on number'
C:\Projects\omaha\omaha/goopdate/com_proxy.h(89): error C2153: integer literals must have at least one digit
C:\Projects\omaha\omaha/goopdate/com_proxy.h(92): error C2059: syntax error: 'bad suffix on number'
C:\Projects\omaha\omaha/goopdate/com_proxy.h(92): error C2153: integer literals must have at least one digit
```

I think most of you would not see this error. When I had an error of `uuidgen`, I statically added generated strings to `proxy_clsid_utils.py`. It caused invalid `proxy_clsids.txt` generation. Then that txt file isn't refreshed after I fix `uuidgen` error.
If you meet this kind of error, erase `omaha/proxy_clsids.txt`.

### Unit tests
After build completes, we can run unit tests.
To run, some settings are needed. Check [this](https://github.com/google/omaha/blob/master/doc/DeveloperSetupGuide.md#running-unit-tests).

On my local machine, 26 tests are failed.
![image](https://user-images.githubusercontent.com/6786187/39614408-9fe62598-4faa-11e8-99b0-9ccf427a0b75.png)

## Omaha Overview
Most of contents comes from [Omaha docs](https://github.com/google/omaha/blob/master/doc/OmahaOverview.html).

The Omaha project provides a shared auto-update and install system for Windows client products at Google that works on multiple Windows platforms, including Windows Vista.

#### Objectives
* One auto-update mechanism shared by many product teams
* A tiny meta-installer which includes the update client(ex, GoogleUpdate.exe) and a reference(tag) to the desired application which the update client can then download and install
* One-click web install of application once the update client is installed
* Support for rich update deploy logic
* Crash reporting

#### Background
* Avoid unnecessary duplication of code and avoid maintaining multiple servers that perform identical functionality from each product team
* Windows Vista features a strict security model that limits the ability of most applications on a machine to perform system-changing activities
* Deploy a rich set of autoupdate capabilities(ex, multiple update tracks) to all client applications
* To address the goals of Vista compatibility, reduction of duplicative effort at Google, improvement of deploy and autoupdate capabilities, and application install user experience improvement, we proposed to develop shared client infrastructure that handles all installation and autoupdating tasks for Google Windows client products. This client communicates with a single Google autoupdate server. Taken together, this server and client is named Google Update. The project code name is **Omaha**. 


### High level scenario(ex, Chrome)

#### Three principal parts
* Client-side run time
* Meta-installer which simply wraps the client run time and either the application installer or a micro reference to the application
* Server

For first install, user downloads an metainstaller called `ChromeSetup.exe` from chrome download site. It's actually the Omaha meta installer with data tag(micro reference to Chrome). Execution of `ChromeSetup.exe` installs Omaha. Then, Omaha reads data tag and Chrome installer is downloaded and executed on the PC. After that, Omaha periodically checks for updates of Chrome.

For secondary install, [document](https://github.com/google/omaha/blob/master/doc/OmahaOverview.html) said that google website detects that Omaha client is installed, and instead of delivering a downloadable EXE or MSI such as `ChromeSetup.exe`, it vends an application micro-reference via an ActiveX object. In this case, download should start instead of going through any complicated download steps.

In the document, there are three mode when user clicks download link in google app download page.
* None
  * Browser provides tagged metainstaller
* OneClick mode (Browser detects Omaha is installed)
  * Browser launches Omaha directly
* ClickOnce mode (.NET ClickOnce is available)
  * Browser launches ClickOnce, which downloads metainstaller and launches it with tag.

When I tested in chrome download page, it seems only *None* mode is used. That means, browser always downloaded *tagged metainstaller* called `ChromeSetup.exe` instead of using *OneClick* mode. When I execute `ChromeSetup.exe`, UAC dialog told that `GoogleUpdateSetup.exe` with tags wants to change my device.

Q1) **Still confusing... what is metainstaller? ChromeSetup.exe or GoogleUpdateSetup.exe?**<br>
A1) As [TaggedMetainstaller](https://github.com/google/omaha/blob/master/doc/TaggedMetainstallers.md) said, `GoogleUpdateSetup.exe` is metainstaller and `ChromeSetup.exe` is tagged metainstaller. As you can see in the below Tagging subsection, they are almost identical except tagged metainstaller store more data in PE Certificates Directory.

Q2) **Why and how `GoogleUpdateSetup.exe` is used instead of `ChromeSetup.exe` by UAC? I executed `ChromeSetup.exe` not `GoogleUpdateSetup.exe`.**<br>
A2) When user executes `ChromeSetup.exe`, it extracts installable files from payload.dll to temp dir. After that, it copies itself to that temp dir as `GoogleUpdateSetup.exe`. Then, it tries to spawns new process with admin `GoogleUpdateSetup.exe` if `ChromeSetup.exe` runs in user mode.

Q3) **GoogleUpdateSetup.exe is the installer of Omaha client?**<br>
A3) Right. it installs Omaha client on local machine.

#### Tagging

Tag bytes are stored in the PE "Certificates Directory" by `ApplyTag` tool.<br>
`ApplyTag` tool usage is `ApplyTag <signed_file> <outputfile> <tag> [append]`.<br>
It adds tag byte to the Certificates Directory in the *signed_file* and rename it to *outputfile*.<br>
For test, I created **BraveSetup.exe** by `ApplyTag GoogleUpdateSetup.exe BraveSetup.exe "appguid={C38B7539-CD23-4954-BDC8-FCE21B2543AE}&appname=Brave%20Browser&needsadmin=prefers&lang=en"`.<br>
When I execute `BraveSetup.exe`, it shows same update process and UI as `ChromeSetup.exe` did.

For more information about Tag, see `apply_tag.cc`, `apply_tag_tool.cc` and `extractor.cc` files in Omaha project.


### Client
The Omaha client is a Windows application, installed by a custom Windows installer.
It performs:
* Installs and updates applications for the host machine
* Periodically talks to the update server to determine whether an update is necessary
* Provides a simple progress UI to notify users that the initial installation of the Omaha client is happening
* The elevation functionality in the Omaha client allows applications to update themselves even if they are running in low-rights mode
* Provides infrastructure for applications to report crashes

When last application using Omaha client is uninstalled by the user, Omaha client will uninstall itself.

#### Client runtime
The runtime functionality is divided between a couple of processes for robustness, running code with the least privilege and resource management.
They consisted with a core process and many worker process.

The execution model for the runtime code depends on how Omaha is actually installed.

* Installed for the machine(all users) - This requires administrator privileges. Windows service runs all the time as SYSTEM and it kicks off worker processes. The worker process runs as SYSTEM as well.
* Installed for the current user - No service and no machine updates. This model solely relies on *goopdate worker process* that runs all the time in each interactive session as the user.

The process execution model is almost identical, in both machine and user cases. For the most part, the machine and user instances of Omaha run in isolated security contexts and they share nothing.

Q1) **What is** *Windows service* **? is it core process?**
A) That is the admin previledged process that runs in background with pre-defined policies and it's not core process.

Q2) **In current user mode, goopdate worker process always runs to check update without core process?**
A) Yes, that worker process runs by registered task scheduler in that mode.

Q3) *goopdate* **is the name of worker process? Or only used in user mode?**
A) *goopdate* is library name that implemented whole omaha features.

The Omaha core runs in a local system process for the machine, and one process for each logged in user. Typically, there will be two core processes running.

The machine Omaha core process is started at boot time by a system service that terminates soon after. The user Omaha core process is started by the shell from the user's run key.

Q1) **What is system service that starts machine Omaha core process?**
A) The registered BraveSoftwareUpdateTaskMachineCore task starts core process.

Q2) **What is user Omaha core process and how execute it?**
A) 

In my system, **Google Update Core** is registered as startup task(Checked by task manager's Startup tab) and *GoogleUpdateCore.exe* is located in `C:\Users\simon\AppData\Local\Google\Update\1.3.33.7`.

It's source code is in `omaha/core`.

Q1) **Who registers it as as startup task?**
A) *GoogleUpdateSetup.exe* registers task that executes *GoogleUpdateCore.exe* in startup task.


# Customization
To use Omaha by brave, manual customization is needed.

Omaha customization repo is https://github.com/simonhong/omaha/tree/brave_omaha

### Create tagged metainstaller
For Omaha installation test, we need to make tagged metainstaller by using `ApplyTag.exe`.

After build with above repo, execute below. appguid in tag is just example id. It should be replaced brave guid later.

Below is for making installer for `stable` with `en` language.
This `BraveBrowserSetup.exe` will download installer from stable channel of `{AFE6A462-C574-4B8A-AF43-4CC60DF4563B}` app.

`ApplyTag.exe BraveUpdateSetup.exe BraveBrowserSetup.exe "appguid={AFE6A462-C574-4B8A-AF43-4CC60DF4563B}&appname=Brave-Browser&needsadmin=prefers&lang=en"`

Below is for making installer for `dev` channel.
This `BraveBrowserDevSetup.exe` will download installer from `dev` channel of `{CB2150F2-595F-4633-891A-E39720CE0531}` app. Value of `ap` should be same as targeted channel name.

`./ApplyTag.exe BraveUpdateSetup.exe BraveBrowserDevSetup.exe "appguid={CB2150F2-595F-4633-891A-E39720CE0531}&appname=Brave-Browser-Dev&needsadmin=prefers&lang=en&ap=dev"`

Also, `install` and `update` action events should be added with `--chrome-dev` argument when adding a new version.

*ApplyTag.exe* is in scones-out/dbg-win/obj/tools/ApplyTag.

### Intall BraveUpdate update client
Execute `BraveBrowserSeup.exe`.

You can see Dialog with "Unable to connect to the Internet...". I assume that invalid update server and appid in installer.

#### Admin mode
It installs itself in `.../Program Files (x86)/BraveSoftware/Update`

Two services are registered.
* Brave Update Service(brave)
* Brave Update Service(bravem)

Two tasks are registered.
* BraveUpdateTaskMachineCore
* BraveUpdateTaskMachineUA

Registry key: `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\BraveSoftware\Update`.

#### User mode
It installs itself in `.../AppData/Local/BraveSoftware/Update`.

Startup task is registered.
* Brave Update

Two tasks are registered.
* BraveUpdateTaskUserS-XXXCore
* BraveUpdateTaskUserS-XXXUA

Registry key: `HKEY_CURRENT_USER\Software\Brave\Update`.

# Test omaha server

See [Crystalnix tutorial](https://github.com/Crystalnix/omaha-server#setting-up-a-development-server).

# Update omaha itself

Build updated `omaha/VERSION` and upload `BraveUpdateSetup.exe` with new version.

Make sure to add additional `update` Action with `/update` arguments.

When update finished, new version's folder is created in `BraveSoftware/Update`.

The shell program(`BraveUpdate.exe`) would not be replaced with newer version because
it just load `goopdate.dll` in new folder. So, new `goopdate.dll` is still compatible with old shell,
old shell is reused.

# Test silent intaller (brave_intaller.exe)

`brave_installer.exe` installs user mode stable version

`brave_installer.exe --system-level` installs admin mode stable version

`brave_installer.exe --system-level --chrome-dev` installs admin mode dev version

`brave_installer.exe --chrome-sxs` installs user mode nightly verion

`brave_installer.exe --chrome-beta` installs user mode beta version
