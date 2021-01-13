Before releasing on the release channel, we need to be able to test upgrades. Omaha has a test channel that we can use and this page has those details (NOTE: x86 and x64 use different channels). macOS uses Sparkle (works similar to Omaha) 

(for overall Omaha information, see [Brave-omaha](https://github.com/brave/brave-browser/wiki/Brave-omaha))

## Upgrading on Windows
Install the latest publicly available release version. Login to a few sites and create a session.

When ready to do the upgrade, quit Brave and download the stub binaries for your platform (x86 or x64) from https://drive.google.com/drive/u/0/folders/1OX1MvNdPMrFggAOkKPGKORjrFeBzsyFF. You can then install this on top of your existing installation.

## Upgrading on macOS
Install the latest publicly available release version. Login to a few sites and create a session.

On macOS, quit Brave and then you can edit the application's `Info.plist` file:
Search for SUFeedURL:
```xml
<key>SUFeedURL</key>
<string>https://updates.bravesoftware.com/sparkle/Brave-Browser/stable/appcast.xml</string>
```

To "get onto" the test channel using `x64 (Intel)`, you can change the above to the following:
```xml
<key>SUFeedURL</key>
<string>https://updates.bravesoftware.com/sparkle/Brave-Browser/test/appcast.xml</string>
```

To "get onto" the test channel using `Arm64`, you can change the above to the following:
```xml
<key>SUFeedURL</key>
<string>https://updates.bravesoftware.com/sparkle/Brave-Browser/test-arm64/appcast.xml</string>
```

You can now re-launch into Brave and use the menu `Brave` => `About Brave` (or visit brave://settings/help) and it should check for an update on the test channel using Sparkle.