## Social Blocking

If you navigate to `brave://settings/socialBlocking`, you will see an option to enable `Allow Google Login buttons on third party sites`.

![social-blocking](https://jumde.github.io/img/google_login_social_blocking.png)

**When this option is enabled:** It adds a third-party cookie exception for accounts.google.com so sites using `Login with Google` can work correctly.

**Starting 1.51**, this option and cookie exception will go away and Brave will prompt per-website when it detects that the website is trying to use third-party cookies for Google Sign-In.

## Extensions

If you navigate to `brave://settings/extensions`, you will see an option to enable `Allow extensions to use chrome.identity api`.

![extensions](https://jumde.github.io/img/google_login_for_extensions.png?123)

**When this option is enabled:** It enables `chrome.identity` for extensions so extensions can retrieve an OAuth token from google to authenticate users. The OAuth token can be used to retrieve personal information like email id, profile. Please note that this is considered a 3rd party login. Extensions such as Google Keep may require a 1st party login (performed in Google Chrome) and may not work ([see here for more information](https://github.com/brave/brave-browser/issues/15754#issuecomment-920514585)). You can read more about `chrome.identity` here: https://developer.chrome.com/apps/identity

If you are not logged into Google, these options have no effect for you.