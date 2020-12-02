You can check the list of components installed in Brave Browser by navigating to `brave://components`:

| Component                 | Supported By Brave     | What it does  | Repo      |Comments      |
| ------------------------- | ---------------------- | ------------- | ------------- | ------------- |
| Autofill States Data      | No           | Contains a mapping of acronyms of state names in different countries to the full names. Helps ease autofilling of state data. | |   |               |
| Brave Ad Block Updater    | Yes | Updates the ad-block lists supported by Brave regularly | https://github.com/brave/adblock-rust |  |
| Brave HTTPS Everywhere Updater      | Yes           | Maintains the HTTPSEverywhere database which is used to upgrade insecure navigations to secure when the matching rules are found | https://github.com/brave/https-everywhere-builder | |
| Brave Local Data Updater  | Yes                    | Used to update AutoplayWhitelist, ExtensionWhitelist, Greaselion scripts and ReferrerWhitelist  |  https://github.com/brave/autoplay-whitelist, https://github.com/brave/referrer-whitelist, https://github.com/brave/extension-whitelist, https://github.com/brave/brave-site-specific-scripts             | |
| Brave SpeedReader Updater      | Yes           | Maintains data files to support Brave Speedreader | https://github.com/brave-experiments/SpeedReader | |
| Brave Tor Client Updater      | Yes           | Contains the Brave tor client required to support Tor windows | https://github.com/brave/tor_build_scripts/ |
| Brave User Model Installer      | Yes           | Maintains data files to support Brave Ads  | https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageClientModelParameterComponent.js | |
| Certificate Error Assistant      | No           | Helps fix errors in misconfigured SSL certificates | | Not able to trigger Certificate Error Assistant in Chrome or Brave              |
| CRLSet      | Yes           | Maintains a list of websites with bad certificates so that users can be protected from harmful websites  |               |
| Crowd Deny      | No           | Revokes all permissions for sites in the list | | Work to enable tracked here: https://github.com/brave/brave-browser/issues/10280 |
| Federated Learning of Cohorts      | No           | Has data to support FLOC | | This feature has significant privacy risks and should not be enabled in Brave |
| File Type Policies      | Yes           | List of file extensions and how they are handled in download protection | | This is used by Safe Browsing |
| Legacy TLS Deprecation Configuration | No           | This component adds a whitelist of domains that can use deprecated TLS 1.0/1.1 components.  |               |
| MEI Preload      | Yes           | Used to pre-load media from websites |               |
| NTP Sponsored Images      | Yes           | Updates the NTP Sponsored image assets |https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageNTPSponsoredImagesComponents.js |
| NTP Super Referral      | Yes           | Updates the NTP Super Referral image assets | https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageNTPSuperReferrerComponent.js |
| NTP Super Referral mapping table | Yes           | Manage NTP SR mapping table (promo code and NTP SR Component id | https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageNTPSuperReferrerMappingTableComponent.js|
| youtubedown.js | Yes           | Used to get youtubedown.js script | https://github.com/brave/brave-core-crx-packager/blob/master/scripts/packageYoutubedown.js |
| Origin Trials      | No           | Metadata for origin trials |               |
| Safety Tips      | No           |  Protobuf of whitelisted domains for client side lookalike URL detection | | Work to enable safety tips is tracked here: https://github.com/brave/brave-browser/issues/12999 |
| Subresource Filter Rules      | No           | Contains rules to block websites which don’t follow the better Ads Standard. Its often used to block phishing domains |               |
| Widevine      | Yes           | Widevine’s DRM solution provides the capability to license, securely distribute and protect playback of content on any consumer device |           |
| Zxcvbn Data Dictionaries      | No           | This contains the data like popular English words and names to higlight password strength.    | | The work to develop a custom password check is being tracked here: https://github.com/brave/brave-browser/issues/12001 |