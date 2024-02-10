If you navigate to `brave://settings/extensions` on Desktop, you will see an option to enable `Allow Google login for extensions`.

![extensions](https://jumde.github.io/img/google_login_for_extensions.png?123)

**When this option is enabled:** It enables `chrome.identity` for extensions so extensions can retrieve an OAuth token from google to authenticate users. The OAuth token can be used to retrieve personal information like email id, profile. Please note that this is considered a 3rd party login. Extensions such as Google Keep may require a 1st party login (performed in Google Chrome) and may not work ([see here for more information](https://github.com/brave/brave-browser/issues/15754#issuecomment-920514585)). You can read more about `chrome.identity` here: https://developer.chrome.com/apps/identity

If you are not logged into Google, these options have no effect for you.