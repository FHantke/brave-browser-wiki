# Switches

In addition to supporting all Chromium command line switches, the following switches are supported:

- `npm start --disable_brave_extension`: Disable loading Brave extension on startup (note: this only disable the extension control view, not Shields)
- `npm start --disable_pdfjs_extension`: Prevent installing the PDFJS extension
- `npm start --disable_webtorrent_extension`: Disable loading Brave WebTorrent extension on startup

> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

## Sponsored New Tab Page
If a CLI flag `--ntp-branded-data-path` is provided with a valid FS directory path, then that directory will be used and the remote component will not be used or fetched.

This is a great way to create a folder with a `photo.json` to test new integrations with the new tab page.

## Useful Chromium switches which must be passed to the binary

- `--show-component-extension-options`
- `--simulate-outdated` simulate Brave being out of date (should show downloading in brave://settings/help)