How to test:
- Official builds
- Release builds done manually
- Debug builds, but for assertions make sure to take Debug only things with a `debug` label.'

Triage:
- Things are triaged into milestones, and assigned an owner by bbondy for now and by a triage meeting eventually.  There is a project that shows per user views of tasks.

Component updater:
- Test that components come from our server and not Chrome's server.

Flash:
- Flash has no UI if it is not installed.  If installed, Flash is disabled by default, you can enable in the urlbar which enables click to play.

Extensions:
- Extensions are restricted, but PWAs are allowed.
- Test to make sure DAT files get updated periodically.
- There is a whitelist of extensions, things not in the allowed list should be disabled.
- The plan is to allow 1k+ extensions initially.
- Extensions can be installed from extensions.brave.com

Enabled/disabled features over Chromium:
- Enabled progressive web apps flag are enabled. (mobile.twitter.com, login and you can install Twitter Lite app).
- Emoji picker control when right clicking in an input text control.
- Tab muting is enabled by default (icon clickable in tabs)
- Sign into Chrome/Brave feature disabled
- Restoring last tab value default pref value set to remember last session
- Search suggestions are disabled by default and replaced with a top sites provider for alexa top 500.

Tests:
- Tests: We are currently running our own browser tests and unit tests, we are not currently running Chromium unit and browser tests, but we'd like to enable some subset of them that apply.

PDF handling:
- PDF handling via the Chromium PDF Viewer extension is explicitly disabled by blocking the extension ID.
- PDF.js is force installed on startup via a setting that's used for administration management policy. It can't currently be disabled, but if we decide to allow that we can make it part of the recommended extension list instead.
- The page action extension button should not show up.

Keychain:
- I believe we store our passwords in our own keychain, I believe we will ask to unlock the keychain of other apps during imports.  (Check with Garrett and/or Anthony)

Site hacks:
- Site hacks should be reviewed individually, they are managed here: `brave/browser/net/brave_site_hacks_network_delegate_helper.cc`. 
- Shield exceptions are managed here `brave/common/shield_exceptions.cc`

Browser level network delegate helper:
- Network redirects mapping Chrome URLs to Brave ones are done for the browser context here: `brave/browser/net/brave_static_redirect_network_delegate_helper.cc`.

Software updates:
- Sparkle (macOS)
- Omaha (Windows)
- Check with Jarv and Simon Hong for details.

Installers and build signing:
- Installers should be signed.

Custom WebUI `about:` pages:
- Newtab WebUI page (it currently does outgoing network requests to s3). Code exists at `brave/components/brave_new_tab_ui`.
- Welcome page (currently remote, but changing to local before we ship, muon is remote though).
- Payments page `brave/components/brave_payments_ui`
- See `package.json` deps in use.  In particular we use some node modules and webpack them together to create a bundle which is loaded from a pak file.

Cookie blocking:
- Cookie blocking is managed here: `brave/browser/brave_content_browser_client.cc`

Referrer blocking:
- Referrer blocking works by TLD+1 comparisons, and if they are different it will override a referring URL as its own origin.

Fingerprinting protection:
- Talk to Jocelyn for more details

Command line switches:
- Brave specific command line switches exist here: `brave/common/brave_switches.cc`.

Brave specific preferences:
- Brave specific preferences are added here: `brave/common/pref_names.cc`.

Protocol handlers:
- There is a brave:// equivalent to chrome:// but it just forwards to chrome://