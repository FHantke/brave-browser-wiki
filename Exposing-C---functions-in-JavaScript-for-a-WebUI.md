There are two common ways you can expose your C++ to JavaScript:
1. using `web_ui()->RegisterMessageCallback` inside the WebUI ([example](https://github.com/brave/brave-core/blob/1cb5818aa0b70666c6aeea5ea9c06cc4e712171a/browser/ui/webui/settings/brave_appearance_handler.cc#L37-L67))
2. exposing the functionality via extension methods ([example](https://github.com/brave/brave-core/blob/46406ce35d11ac8dba4f1d0dc8f11bfa90286ca0/common/extensions/api/_api_features.json#L96-L104)). Notice that a context can be given for when this would be in scope (`allowlist` for which extension IDs have access, `matches` for which pages have access, etc)

The WebUI can then access that method in JavaScript.
- When using `RegisterMessageCallback`, you would access using `chrome.send`. For example: `chrome.send('setBraveThemeType', 'Dark')`. For situations needing to use a promise, you'd be able to use something like:
    ```
    import {sendWithPromise} from 'chrome://resources/js/cr.m.js';
    // ..
    return sendWithPromise('getBraveThemeType')
    ```
- When using extension methods, you'd use the path you exposed. For example: `chrome.braveTheme.setBraveThemeType('Dark')`

## Which is preferred?
We should always prefer using `RegisterMessageCallback` in a WebUI. Additionally, it we should avoid adding any logic to our Extension API.

An exception can be made for using an extension API on a WebUI page if:
- We need to call the API from an extension page as well (prevent having to duplicate code) AND
- The page isn't / won't be used on Android

If those two criteria can't be met, the WebUI should be using `RegisterMessageCallback` to register methods for use with `chrome.send`

## Related
- For an overview of WebUI, check out [WebUI Explainer](https://chromium.googlesource.com/chromium/src/+/lkgr/docs/webui_explainer.md)
- For how Brave builds WebUI resources, check out [WebUI Builds, Paths and Resources](https://github.com/brave/brave-browser/wiki/WebUI-Builds,-Paths-and-Resources)



