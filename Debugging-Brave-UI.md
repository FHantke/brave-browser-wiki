# Debugging Brave UI

You can use the Chrome remote developer tools to debug aura UI views (Windows and Linux).

1. Run `src/out/Debug/brave --enable-ui-devtools`
2. Run `chromium-browser --enable-ui-devtools` (you can also use Chrome if you prefer)
3. In the Chromium browser, go to `chrome://inspect`
4. Click on Other and in the section entitled "UiDevToolsClient" click on the `inspect` link.

You can now use the DOM inspector and some of the other devtools.

<img width="1712" alt="screen shot 2018-06-10 at 12 50 46 pm" src="https://user-images.githubusercontent.com/831718/41204013-268f97d6-6cad-11e8-80c4-641e7f998aac.png">

## Changing the port

You can also pass `--enable-ui-devtools=<port>` if you want to use a different port.

## Draw View's bounds

You can see each View widget's bounds by passing `--draw-view-bounds-rects`.

![debug_view_ui](https://user-images.githubusercontent.com/6786187/41207426-d58a49cc-6d50-11e8-8eba-e1189ba3dba7.png)
