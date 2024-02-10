### Legacy Google Sign-In

Some websites still use [legacy Google Sign-In](https://developers.google.com/identity/sign-in/web/sign-in) which requires the use of privacy-harmful 3rd party cookies. [Brave blocks third-party cookies by default](https://brave.com/glossary/cookie/#how-can-i-prevent-cookies-from-being-used-to-track-me), but if you try to login to a (rare) website that still requires 3rd party cookies for Google Sign-In, Brave will ask if you want to proceed via a permission prompt. See our [blog post](https://brave.com/privacy-updates/24-google-sign-in-permission/) for more details on this feature.

<img width="635" alt="image" src="https://github.com/brave/brave-browser/assets/5284154/a6372669-95bf-4510-b9d6-ad92852ec1d5">

### Extensions

If you navigate to `brave://settings/extensions` on Desktop, you will see an option to enable `Allow Google login for extensions`.

<img width="674" alt="image" src="https://github.com/brave/brave-browser/assets/5284154/fc0cbc0e-87e1-436b-addd-32e97f3d96cd">


**When this option is enabled:** It enables `chrome.identity` for extensions so extensions can retrieve an OAuth token from google to authenticate users. The OAuth token can be used to retrieve personal information like email id, profile. Please note that this is considered a 3rd party login. Extensions such as Google Keep may require a 1st party login (performed in Google Chrome) and may not work ([see here for more information](https://github.com/brave/brave-browser/issues/15754#issuecomment-920514585)). You can read more about `chrome.identity` here: https://developer.chrome.com/apps/identity.

If you are not logged into Google, these options have no effect for you.