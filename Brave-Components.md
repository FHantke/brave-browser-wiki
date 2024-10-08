# Overview

Component extensions (or, more simply, components) allow Brave to receive asynchronous updates to core functionality without needing to update Brave itself. Currently, Brave uses components to download the data files associated with Ad Block, HTTPS Everywhere, Tracking Protection, Tor Client Updater and Widevine Content Decryption Module and Crypto Wallets.

# Installation and Updates

On startup, Brave installs its registered components via `https://go-updater.brave.com/extensions`. If already installed, Brave verifies that the components are up-to-date. Once installed/verified, Brave checks for new updates every 5 hours.

# Performing an On-Demand Update Check

To perform an on-demand update check of your installed components, visit `brave://components/` in Brave and press the appropriate "Check for update" button.


You can check the list of components installed in Brave Browser by navigating to `brave://components`:

| Component                 | Supported By Brave     | Component ID  | What it does  | Repo          | Comments      |
| ------------------------- | ---------------------- | ------------- | ------------- | ------------- | ------------- |
| Autofill States Data      | No           | AutofillStates | Contains a mapping of acronyms of state names in different countries to the full names. Helps ease autofilling of state data. | |   |               |
| Brave Ad Block Updater    | Yes | iodkpdagapdfkphljnddpjlldadblomo | Contains Brave's default adblock lists | https://github.com/brave/brave-core-crx-packager | Component ID "mined" to include the substring `adblo` |
| Brave Ad Block List Catalog    | Yes | gkboaolpopklhgplhaaiboijnklogmbc | Enumerates all Brave's supported regional list components | https://github.com/brave/adblock-resources/tree/master/filter_lists/list_catalog.json |  |
| Brave Ad Block Resources Library    | Yes | mfddibmblmbccpadfndgakiopmmhebop | Enumerates all Brave's default resource replacements and scriptlets for adblocking | https://github.com/brave/adblock-resources, https://github.com/brave/uBlock |
| Regional lists (e.g. `Easylist-Cookie List - Filter Obtrusive Cookie Notices`)    | Yes | various | Contains any regional lists enabled in brave://settings/shields/filters | https://github.com/brave/adblock-resources/tree/master/filter_lists/list_catalog.json | See `list_text_component` fields for relevant component IDs |
| Brave HTTPS Everywhere Updater      | Yes           | oofiananboodjbbmdelgdommihjbkfag | Maintains the HTTPSEverywhere database which is used to upgrade insecure navigations to secure when the matching rules are found |  https://github.com/brave/https-everywhere-builder | |
| Brave Local Data Updater  | Yes                    | afalakplffnnnlkncjhbmahjfjhmlkal | Used to update Greaselion scripts, etc. |  https://github.com/brave/brave-site-specific-scripts             | |
| Brave SpeedReader Updater      | Yes           | jicbkmdloagakknpihibphagfckhjdih | Maintains data files to support Brave Speedreader | https://github.com/brave-experiments/SpeedReader | |
| Brave Tor Client Updater      | Yes           | cldoidikboihgcjfkhdeidbpclkineef<br/> cpoalefficncklhjfpglfiplenlpccdb<br/>    biahpgbdmdkfgndcmfiipgcebobojjkp<br/>  | Contains the Brave Tor client required to support Tor windows | https://github.com/brave/tor_build_scripts/ |
| Brave User Model Installer      | Yes           | kkjipiepeooghlclkedllogndmohhnhi | Maintains data files to support Brave Ads  | https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageClientModelParameterComponent.js | |
| Certificate Error Assistant      | Yes           | SSLErrorAssistant | Helps fix errors in misconfigured SSL certificates | | |
| CRLSet      | Yes           | CertificateRevocation | Maintains a list of websites with bad certificates so that users can be protected from harmful websites  |               |
| Crowd Deny      | Yes          | Crowd Deny | Revokes all permissions for sites in the list | | |
| Federated Learning of Cohorts      | No           | Floc | Has data to support FLOC | | This feature has significant privacy risks and should not be enabled in Brave |
| File Type Policies      | Yes           | FileTypePolicies | List of file extensions and how they are handled in download protection | | This is used by Safe Browsing |
| Hyphenation | Yes           | Hyphenation | |  Data that assists css-hyphens in chromium  | |
| Legacy TLS Deprecation Configuration | No           | TLSDeprecationConfig | | This component adds a whitelist of domains that can use deprecated TLS 1.0/1.1 components.  | Not needed. We disable TLS 1.0/1.1 completely in Brave. |
| MEI Preload      | Yes           | MEIPreload | Used to pre-seed the Media Engagement Index which determines whether auto-play is enabled on a site | https://www.chromium.org/audio-video/autoplay/autoplay-pre-seeding-in-chrome   |
| NTP Sponsored Images      | Yes           | gccbbckogglekeggclmmekihdgdpdgoe | Updates the NTP Sponsored image assets |https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageNTPSponsoredImagesComponents.js |
| NTP Super Referral      | Yes           | | Updates the NTP Super Referral image assets | https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageNTPSuperReferrerComponent.js |
| NTP Super Referral mapping table | Yes           | | Manage NTP SR mapping table (promo code and NTP SR Component id | https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageNTPSuperReferrerMappingTableComponent.js|
| Origin Trials      | No           | OriginTrials | Metadata for origin trials |               |
| Safety Tips      | Yes         | SafetyTips | Protobuf of domains for client side lookalike URL detection | | |
| Subresource Filter Rules      | No           | Subresource Filter | Contains rules to block websites which don’t follow the better Ads Standard. Its often used to block phishing domains |               |
| Widevine      | Yes           | oimompecagnajdejgnnjijobebaeigek | Widevine’s DRM solution provides the capability to license, securely distribute and protect playback of content on any consumer device |           |
| youtubedown.js | Yes           | | Used to get [playlist-component](https://github.com/brave/playlist-component) | https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageYoutubedown.js |
| Zxcvbn Data Dictionaries      | No           | ZxcvbnData | This contains the data like popular English words and names to higlight password strength.    | | The work to develop a custom password check is being tracked here: https://github.com/brave/brave-browser/issues/12001 |
| Crypto Wallets | Yes | odbfpeeihdkbihmopkbjmoonfanlbfcl | Contains the Brave Crypto Wallet component required to support Dapp | http://github.com/brave/ethereum-remote-client/ |  Component lazy loads by default unless set to run at browser startup