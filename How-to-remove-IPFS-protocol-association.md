Sometimes when you open IPFS links like next one: `ipfs://bafybeiemxf5abjwjbikoz4mc3a3dla6ual3jsgpdr4cjr3oz3evfyavhwq/wiki/Vincent_van_Gogh.html`, Brave can propose you to open the link with other installed Brave version (Ex.: Nightly or may be Development). 

It happens, because IPFS scheme is associated with you Nightly or Development Brave version.
As the IPFS support has been deprecated, the next steps help you to remove registered IPFS scheme association from your OS.
##### MacOS
1. Run the next command: `open ~/Library/Preferences/com.apple.LaunchServices/com.apple.launchservices.secure.plist`
2. Find all items with `LSHandlerURLScheme` equals `ipfs` or `ipns` and delete them.
3. Reboot the computer.

##### Windows
1. Open `Registry Editor`
2. Check `ipfs` or/end `ipns` keys existing in the next registry location `Computer\HKEY_CLASSES_ROOT`
3. If exists, make sure that it is associated with Brave Browser by checking the `Computer\HKEY_CLASSES_ROOT\ipfs\shell\open\command`.
4. Delete whole protocol association: `Computer\HKEY_CLASSES_ROOT\ipfs` and/or `Computer\HKEY_CLASSES_ROOT\ipns`

