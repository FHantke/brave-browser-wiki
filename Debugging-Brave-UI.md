# Debugging Brave UI

You can use the Chrome remote developer tools to debug aura UI views (only available to Windows, Linux and ChromeOS).

## Instructions

1. `cd` to `src/out/Debug`
2. Run `brave.exe --enable-ui-devtools` This will start Brave with the UiDevToolsClient flag enabled.
3. `cd` to the application folder of Chromium or Chrome. Note: This can not be a stable release of Chrome. UI Devtools are [not available in stable releases of Chrome any longer.](https://cs.chromium.org/chromium/src/chrome/browser/about_flags.cc?l=4410)
4. Run `chromium-browser.exe --enable-ui-devtools` This will start Chromium with the UiDevToolsClient flag enabled.
5. In the Chromium browser, type `chrome://inspect` into the URL bar.
4. Click on the `Other` tab and in the section entitled "UiDevToolsClient" click on the `inspect` link.

You can now use the DOM inspector and some of the other devtools.

### Further Reading

1. https://groups.google.com/a/chromium.org/forum/#!topic/chromium-dev/eHeHP4RuvC0
2. https://www.chromium.org/developers/how-tos/inspecting-ash

<img width="1712" alt="screen shot 2018-06-10 at 12 50 46 pm" src="https://user-images.githubusercontent.com/831718/41204013-268f97d6-6cad-11e8-80c4-641e7f998aac.png">

## Changing the port

You can also pass `--enable-ui-devtools=<port>` if you want to use a different port.

## Draw View's bounds

You can see each View widget's bounds by passing `--draw-view-bounds-rects`.

![debug_view_ui](https://user-images.githubusercontent.com/6786187/41207426-d58a49cc-6d50-11e8-8eba-e1189ba3dba7.png)
