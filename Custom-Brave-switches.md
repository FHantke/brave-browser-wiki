# Switches

In addition to supporting all Chromium command line switches, the following switches are supported:

- `npm start --disable_brave_extension`: Disable loading Brave extension on startup
- `npm start --disable_pdfjs_extension`: Prevent installing the PDFJS extension
- `npm start --disable_webtorrent_extension`: Disable loading Brave WebTorrent extension on startup

> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.


## Useful Chromium switches which must be passed to the binary

- `--show-component-extension-options`