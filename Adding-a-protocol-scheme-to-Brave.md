We currently have 3 different types of protocol handlers in Brave:
- External protocol handler used for handling magnet URLs
- External protocol handlers used for oauth flows (cryptocurrency exchanges at the time of writing)
- `brave://` being an alias for `chrome://`


Below is a description of changes and patching we've had to do for schemes:

- `AddAdditionalSchemes` is only used for adding `brave://` to the same list of URLs as `chrome://`. [Example](https://github.com/brave/brave-core/blob/master/common/brave_content_client.cc#L14) 

- `GrantRequestScheme` -  Allows a webui to access it so no reason for that. [Example](https://github.com/brave/brave-core/blob/master/patches/content-browser-child_process_security_policy_impl.cc.patch)

- `RegisterURLSchemeAsNotAllowingJavascriptURLs` - Registers an URL scheme to not allow manipulation of the loaded page by bookmarklets or javascript: URLs typed in the omnibox. [Example](https://github.com/brave/brave-core/blob/master/patches/content-renderer-render_thread_impl.cc.patch)

- `RegisterURLSchemeAsDisplayIsolated` - Registers a URL scheme to be treated as display-isolated. This means that pages cannot display these URLs unless they are from the same scheme. For example, pages in another origin cannot create iframes or hyperlinks to URLs with the scheme. [Example](https://github.com/brave/brave-core/blob/master/patches/content-renderer-render_thread_impl.cc.patch)

- `CanAddURLToHistory` - Prevents a URL from being added to history, note that redirects do not appear in history.  So this is probably not needed for oauth related schemes.  [Example](https://github.com/brave/brave-core/blob/master/chromium_src/chrome/browser/history/history_utils.cc)

- `IsHandledProtocol` - Needed for protocol handlers, but not needed for external protocol handlers.  [Example](https://github.com/brave/brave-core/blob/master/patches/chrome-browser-profiles-profile_io_data.cc.patch)

- ` ExtensionTabUtil::IsKillURL` - This is only needed because `brave://` is an alias for `chrome://` and an explicit check is made. [Example](https://github.com/brave/brave-core/blob/master/patches/chrome-browser-extensions-extension_tab_util.cc.patch)