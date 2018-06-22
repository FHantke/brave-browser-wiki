Started from [Help page](chrome://settings/help) to know how browser update works.

## WebUI update modules

[Help page](chrome://settings/help) shows update progress whether new version download is in progress or finished and relaunch button when update is ready. So that page is good start point to study about how update works on both WebUI and C++ world.

In the source code, the term `about page` is used instead of `help page`.

[about_page.js](https://cs.chromium.org/chromium/src/chrome/browser/resources/settings/about_page/about_page.js) and [about_page.html](https://cs.chromium.org/chromium/src/chrome/browser/resources/settings/about_page/about_page.html) are main resources for that page.

In `about_page.html`, below code shows update status.(Unrelated codes are deleted)

```
<div class="start padded">
  <div id="updateStatusMessage" hidden="[[!showUpdateStatus_]]">
    <div
      inner-h-t-m-l="[[getUpdateStatusMessage_(currentUpdateStatusEvent_)]]">
    </div>
  </div>
  <div class="secondary">$i18n{aboutBrowserVersion}</div>
</div>
```

In above code, `currentUpdateStatusEvent_` variable seems data that holds current udpate status.
How `currentUpdateStatusEvent_` has current update status?
In `about_page.js`, it is set by `onUpdateStatusChanged()` method.
`onUPdateStatusChanged()` is bound with `update-status-changed` event.
That event is emitted from [AboutHandler::SetUpdateStatus()](https://cs.chromium.org/chromium/src/chrome/browser/ui/webui/settings/about_handler.cc?rcl=35fa35716150a4a7a222979e36b6f45e523e68c2&l=647) in  c++ world.

When `help page` is loaded, `StartListening()` in `about_page.js` is run.
Then, `aboutBrowserProxy_.refreshUpdateStatus()` calls `AboutHandler::HandleRefreshUpdateStatus()`.
And platform specific `VersionUpdater::CheckForUpdate()` check current status and makes update at `about page`.

`AboutHandler` is initialized in `MdSettingsUI` ctor.

So far, I saw the WebUI-side update module briefly.
Let's check how `VersionUpdater` is implemented on all platforms.

## Native(C++) update modules
AFAICT, there are four native modules related with update in chrome.

To understand how chrome update work, we need to know about below modules.

* VersionUpdater
* UpgradeDetector
* UI (Infobar, toolbar and app menu)
* BrowserProcessImpl::StartAutoupdateTimer

## VersionUpdater
`VersionUpdater` is interface to expose per-platform updating functionality to webui.
As I wrote above, `AboutHandler` owns `VersionUpdater`. `VersionUpdater::CheckForUpdate()` is most important method. It begins the update process by checking for update availability from help page.

#### Windows

On Windows, chromium has two versions of `VersionUpdater`

* VersionUpdaterWin (chrome brand)
* VersionUpdaterBasic (non chrome brand)

###### VersionUpdaterWin
`VersionUpdaterWin` uses on [`google_upddate_win.h`](https://cs.chromium.org/chromium/src/chrome/browser/google/google_update_win.h) and `UpdateCheckDriver` seems do update works with COM class with `CLSID_GoogleUpdate3WebUserClass`.
`GoogleUpdateCore.exe` includes that coclass and it is built from [omaha](https://github.com/brave/omaha).

`UpdateCheckDriver` notifies update status from `GoogleUpdateCore.exe` to `VersionUpdaterWin` via `UPdateCheckDelegate`. Then, `about page` displays update status/progress via callback passed to `VersionUpdaterWin`.

###### VersionUpdaterBasic
`VersionUpdaterBasic` depends on `UpgradeDetector` to get upgrade status only update is ready or not by checking `UpgradeDetector::GetInstance()->notify_upgrade()`.

###### TODOs
We should implement `brave_update_win.h` that uses our own version of `brave_update_idl.idl`(similar to  `google_update_idl.idl`) to replace `google_update_win.h`. That coclasses would be included by `BraveUpdate.exe` from [omaha](https://github.com/brave/omaha).

#### MacOSX

On MacOSX, [`VersionUpdaterMac`](https://cs.chromium.org/chromium/src/chrome/browser/ui/webui/help/version_updater_mac.h) is used to check update status with [`KeystoneGlue`](https://cs.chromium.org/chromium/src/chrome/browser/mac/keystone_glue.h). `KeystoneGlue` uses `KeystoneRegistration.framework`.
It's not open-sourced and used via `NSBundle`.

`KeystoneRegistration.framework` seems to update status via `NSNotificationCenter` and `KeystoneGlue` observers its messages(`KSRegistrationXXXNotification`).
With those mesages, `KeystoneGlue` manages udpate status and propagates udpate status to chrome by sending `kAutoupdateStatusNotification` via noti center.

If `KeystoneRegistration.framework` doesn't exist on local machine, `VersionUpdaterMac::CheckForUpdate` do nothing. Just notify that update is disabled.

`VersionUpdaterMac` gets update status from `kAutoupdateStatusNotification`, then this update status is used by `about page`.

There is another client of `kAutoupdateStatusNotification`. It's [`KeystonePromotionInfoBar`](https://cs.chromium.org/chromium/src/chrome/browser/ui/cocoa/keystone_infobar_delegate.mm?l=108). It will be covered in UI section later.

###### TODOs

`Sparkle.framework` will be used for update on MacOSX. We should implement like `BraveVersionUpdateMac` that interact with `SparkleGlue`.

#### Linux
On Linux, `VersionUpdateBasic` is included. See above `VersionUpdaterBasic` section.

## UpgradeDetector(Impl)

`UpgradeDetector` monitors whether new version is installed in background.
It periodically checkes whether new version is installed on machine by comparing current running version and installed version by `UpgradeDetectorImpl::DetectUpgradeTask`.
When it detects, notifies upgrade recommendation to observers.(see below the observer list)

The way to detect installed version varies on platform.

On windows, register key/value (by `InstallUtil::GetChromeVersion`) is used to detect.

On MacOSX, the value of `CFBundleShortVersionString` in Info.plist is used. (See `keystone_glue::CurrentlyInstalledVersion()`)

On Posix(Linux), new version is obtained by creating new browser process instance with `switches::kProductVersion`. (See `base::GetAppOutput()`).
 
If detect interval isn't specified in cmdline, default interval (in [`upgrade_detector_impl.cc`](https://cs.chromium.org/chromium/src/chrome/browser/upgrade_detector_impl.cc)) is 2 hours.
`constexpr base::TimeDelta kCheckForUpgrade = base::TimeDelta::FromHours(2)`

So, `UpgradeDetector` detects whether Update module/process(such as GoogleUpdateCore.exe(Win), Keystone(MacOSX) or package manager(Linux?)) downloads new version or not and notifies based on upgrade severity.

There are many `UpgradeObservers`.

* ServiceProcessControl propagate update status to service processes
* AppMenuIconController controls whether show update icon over appmenu
* RelaunchNotificationController controls relaunch bubble ui based on annoyance level.
* ToolbarView controls whether update bubble notification shows or not
* AboutHandler triggers update check when help page is visible

##### TODOs

We can use most of existing logics except checking installed version.
We might use different register key on Win and use SparkleGlue to check installed version on MacOSX.

## UI

I found that there are three UIs related with update.

* KeystonePromotionInfobar (show info bar when promotion is enabled)
* ToolbarVew (notification bubble)
* App menu (update menu entry with icon)
* App menu icon (app menu button shows update status icon)

##### TODOs

I think most of UIs can be reused except `KeystonePromotionInfobar`

## BrowserProcessImpl::StartAutoupdateTimer

This timer trigger restarts when upgrade is ready and chromium is running in background.

## Brief summary

Chromium uses different external modules(GoogleUpdateCore.exe, Keystone and package manager) to install new version.

Chromium detects upgrade by checking installed version vs running version.

When chromium determines to recommand upgrade to user, infobar, app menu and notification bubble are used.

Also, when user opens help page, it triggers update check.