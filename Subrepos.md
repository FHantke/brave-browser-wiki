The [`brave/brave-browser`](https://github.com/brave/brave-browser) repo depends on other repositories.  The paths are relative to the root clone of this repo:

- [Chromium](https://chromium.googlesource.com/chromium/src.git)
  - Fetches code via depot_tools.
  - sets the branch for Chromium (ex: 65.0.3325.181).
- [brave-core](https://github.com/brave/brave-core)
  - Mounted at `src/brave`.
  - Maintains patches for 3rd party Chromium code.
- [brave-extension](https://github.com/brave/brave-extension)
  - Mounted at `src/brave/vendor/brave-extension`.
  - Browser action extension which implements the UI for the shields panel.
- [ad-block](https://github.com/brave/ad-block)
  - Mounted at `src/brave/vendor/ad-block`.
  - Implements Brave's ad-block engine.
- [tracking-protection](https://github.com/brave/tracking-protection)
  - Mounted at `src/brave/vendor/tracking-protection`.
  - Implements Brave's tracking-protection engine.