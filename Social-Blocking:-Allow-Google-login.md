If you navigate to `brave://settings/socialBlocking`, you will see an option to enable `Allow Google Login in extensions and third party`.

![social-blocking](https://jumde.github.io/img/social_blocking.png)

When this option is enabled:

1. It adds a third-party cookie exception for accounts.google.com so sites using `Login with Google` can work correctly.
2. It enables `chrome.identity` for extensions so extensions like Google Keep and Google Calendar can retrieve an OAuth token from google to authenticate users. The OAuth token can be used to retrieve personal information like email id, profile. You can read more about chrome.identity here: https://developer.chrome.com/apps/identity

If you are not logged into Google, this option has no effect for you.