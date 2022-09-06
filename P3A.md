# Privacy Preserving Product Analytics Metrics
This is a human-readable list of product analytics metrics collected.
The list of metrics collected can also be found [in the source
code](https://github.com/brave/brave-core/blob/master/components/p3a/brave_p3a_service.cc#L67).
The analytics reported are not exact numbers, but are collected into aggregate
"buckets" or histograms. The process for this aggregation can also be verified [in
the source code](https://github.com/brave/brave-core/blob/cbfc3c2abceabf14e3528a558b7e0aa7378bb1c1/components/p3a/brave_histogram_rewrite.cc#L24).


## Current Metrics

**[D]** = supported on Desktop ([merged and enabled in 1.1.x](https://github.com/brave/brave-core/pull/3242))

**[A]** = supported on Android ([merged in 1.19.x](https://github.com/brave/brave-core/pull/7016) and [enabled in 1.20.x](https://github.com/brave/brave-core/pull/7550))

### Q1: How long has this browser been open for the last seven days?
_`Brave.Uptime.BrowserOpenMinutes`_ **[D]**
1. N/A -- seven days have not passed since first open
2. 0 - 30min
3. 30min - 5hrs
4. 5hrs+

### Q2: Have you made Brave your default browser?
`_Brave.Core.IsDefault_` **[D]**
1. No
2. Yes

### Q3: Did you follow the on-boarding process
_`Brave.Welcome.InteractionStatus`_ **[D]**
1. Did not click within welcome screen
2. Clicked once 
3. Clicked twice or more but did not make it to the end
4. Made it to end of on-boarding flow

### Q4: Did you import settings and bookmarks, and if so from where?
_`Brave.Importer.ImporterSource`_ **[D]**
1. Did not import
2. Imported from Brave
3. Imported from Chrome
4. Imported from Firefox
5. Imported from Bookmarks HTML
6. Imported from Safari (answer only possible on macOS)

### Q5: How many bookmarks do you have?
_`Brave.Core.BookmarksCountOnProfileLoad.2`_ **[D]** **[A]**
1. 0-5
2. 6-20
3. 21-100
4. 101-500
5. 501-1000
6. 1001-5000
7. 5001-10000
9. More than 10000

### Q6: How many open windows do you have?
_`Brave.Core.WindowCount.2`_ **[D]**
1. 0
2. 1
3. 2-5
4. 6+

### Q7: How many open tabs do you have?
_`Brave.Core.TabCount`_ **[D]** **[A]**
1. 0-1
2. 2-5
3. 6-10
4. 11-50
5. 50+

### Q8: What has been your interaction with the shields icon?
_`Brave.Shields.UsageStatus`_ **[D]**
1. Never clicked on shields icon
2. Clicked on it, but never made a change
3. Clicked on it, shut off shields for one or more sites
4. Clicked on it, changed Shields defaults or per-site shield settings in addition to (or in place of) shutting off shields on one or more sites

### Q9: Have you ever used a private window?
_`Brave.Core.LastTimeIncognitoUsed`_ **[D]**
1. Used in last 24h
2. Used in last week but not 24h
3. Used in last 28 days but not week
4. Ever used but not in last 28 days
5. Never used

### Q10: Have you ever used a Tor private window?
_`Brave.Core.TorEverUsed`_ **[D]**
1. Yes
2. No

### Q11: What is the state of your Brave Rewards wallet? 
_`Brave.Rewards.WalletState`_ **[D]** (ANDROID?)
1. No wallet created
2. Wallet created; no grants claimed; no funds added
3. Wallet created; grant(s) claimed; no funds added
4. Wallet created: no grants claimed; funds added
5. Wallet created: grant(s) claimed; funds added
6. Wallet disabled after having been created (Brave Rewards switched off)

### Q12: How much BAT, excluding grants, is in your wallet?
_`Brave.Rewards.WalletBalance.3`_ **[D]** **[A]**
1. No wallet created
2. Wallet created, rewards disabled
4. Zero BAT, excluding grants
3. 1-9 BAT, excluding grants
4. 10-50 BAT, excluding grants
5. Over 50 BAT, excluding grants~~

### Q13: Have you made use of Auto-contribute in Brave Rewards?
_`Brave.Rewards.AutoContributionsState.2`_ **[D]** **[A]**
1. No wallet created
2. Rewards Disabled
3. Wallet created, Auto-Contribute off
4. Auto-contribute enabled, no contribution so far
5. Auto-contribute enabled, one successful contribution so far
6. Auto-contribute enabled, more than one successful contribution so far

### Q14: Have you made use of tips within Brave Rewards?
_`Brave.Rewards.TipsState.2`_ **[D]** **[A]**
1. No wallet created
2. Rewards Disabled
3. Wallet created, no tips sent
4. Wallet created, one-time tip sent
5. Wallet created, at least one monthly tip queued up or sent
6. Wallet created, one-time sent and at least one monthly tip queued up or sent

### Q15: Have you enabled sync?
_`Brave.Sync.Status`_ **[D]**
1. No
2. Sync enabled with two devices
3. Sync enabled more than two devices

### Q16: How many extensions have you installed?
_`Brave.Core.NumberOfExtensions`_ (only queried on program start) **[D]**
1. None
2. One extension
3. Two to four extensions
4. Five or more extensions

### Q17: Have you enabled Brave Ads? **[BROKEN]**
_`Brave.Rewards.AdsState.2`_ **[D]** **[A]**
1. No wallet created
2. Rewards Disabled
3. Wallet created, ads not enabled
4. Wallet created and ads enabled 
5. Wallet created, ads enabled, then disabled, while Brave Rewards remains on
6. Wallet created, ads enabled, then disabled, Brave Rewards disabled

### Q18: How many questions the browser was able to answer within a week?
_`Brave.P3A.SentAnswersCount`_ **[D]** **[A]**
1. None
2. Between 1 and 4
3. Between 5 and 9
4. 10 or more

### Q19: How many times did you search last week?
_`Brave.Omnibox.SearchCount`_ **[D]**
Note: we count only omnibox events that trigger a search.

1. Never
2. 1 to 5 times
3. 6 to 10 times
4. 11 to 20 times
5. 21 to 50 times
6. 51 to 100 times
7. 101 to 500 times
8. More than 500 times 

### Q20: Which is your currently selected search engine? 
_`Brave.Search.DefaultEngine.4`_ **[D]** **[A]**

0. Other
1. Google
2. DuckDuckGo
3. Startpage
4. Bing
5. Qwant
6. Yandex
7. Ecosia
8. Brave Search

### Q21: How much data did Brave save you last week?
Note: this is based on a predictive algorithm running strictly client-side

`Brave.Savings.BandwidthSavingsMB` **[D]** **[A]**

1. 0
2. \>0-50mb
3. 51-100mb
4. 101-200mb
5. 201-400mb 
6. 401-700mb
7. 701-1500mb
8. more than 1501mb

### Q22: Is crash reporting enabled?
Note: Records the corresponding preference (`"Help improve Brave's features and performance
"`) state during a startup.

`Brave.Core.CrashReportsEnabled` **[D]** **[A]**

1. no
2. yes

### Q23: Have you used SpeedReader?

`Brave.SpeedReader.Enabled` **[D]**

1. Never Used
2. Used but not recently (in reporting interval)
3. Used within the last week

### Q24: Number of times user clicked on SpeedReader button to toggle the feature?

`Brave.SpeedReader.ToggleCount` **[D]**

1. 0
2. 1-5
3. 5-10
4. 10-20
5. 20-30
6. 30+

### Q25: On average, how many New Tab Pages did you create per day?

`Brave.NTP.NewTabsCreated` **[D]** **[A]**

0. 0 
1. 1 to 3
2. 4 to 8
3. 9 to 20
4. 21 to 50
5. 51 to 100
6. More than 100


### Q26: Is the sponsored new tab page option enabled?

`Brave.NTP.SponsoredImagesEnabled` **[D]** **[A]**

0. Disabled
1. Enabled

### Q27: On average, how many of the New Tab Pages you saw per day were sponsored?

`Brave.NTP.SponsoredNewTabsCreated` **[D]** **[A]**

0. 0%
1. &gt; 0% and < 10%
2. 10% to < 20%
3. 20% to < 30%
4. 30% to < 40%
5. 40% to < 50%
6. 50% or more

### Q28: On the New Tab Page, did you click the Customize Settings icon?

`Brave.NTP.CustomizeUsageStatus` **[D]**

0. No
1. Yes, but I didn't make any changes
2. Yes, and I did make a change


### Q29: Is IPFS Companion installed?

`Brave.IPFS.IPFSCompanionInstalled` **[D]**

0. No
1. Yes

### Q30: How many lifetime times are IPFS detection prompts shown without installing

`Brave.IPFS.DetectionPromptCount` **[D]**

0. 0 times
1. 1 time
3. 2-5 times
4. 5+ times or more

### Q30: How does IPFS resolve ipfs/ipns protocol?

`Brave.IPFS.GatewaySetting` **[D]**

0. Ask
1. Gateway
2. Local Node
3. Disabled


### Q31: How long did the daemon run?

`Brave.IPFS.DaemonRunTime` **[D]**

0. 0-5min
1. 5-60min
3. 1h-24h
4. 24h+

### Q32: Have you ever used Brave Today?

`Brave.Today.HasEverInteracted` **[D]**

0. Yes

### Q33: How many times did you use Brave Today in the last week that Brave Today was used?*

`Brave.Today.WeeklySessionCount` **[D]**

0. Never
1. Once
2. 2-3 times
3. 4-7 times
4. 8-12 times
5. 13-18 times
6. 19-25 times
7. 26+ times

### Q34: How many articles did you visit (or read) in your largest article-reading session in the preceeding week of the last time you visited a Brave Today article?*

`Brave.Today.WeeklyMaxCardVisitsCount` **[D]**

0. None _(won't ever be sent)_
1. 1
2. 2-3
3. 4-6
4. 7-10
5. 11-15
6. 16+

### Q35: How many rows of articles did you view in your largest row-viewing session in the preceeding week of the last time you used Brave Today?*

`Brave.Today.WeeklyMaxCardViewsCount` **[D]**

0. None _(won't ever be sent)_
1. 1
2. 2-4
3. 5-12
4. 13-20
5. 21-40
6. 41-80
7. 81+

_* Refers to the issue whereby stats won't be reset or updated if the corresponding feature is not used in the next time period._

### Q36: Was reset sync progress marker procedure ever done?
_`Brave.Sync.ProgressTokenEverReset`_ **[D]** **[A]**
1. 0-Normal reset of progress marker due to 7th failure
2. 1-Reset progress marker due to 7th failure happening twice in a row in less than 30 minutes

### Q37: Have you used the wallet in the previous month?
_`Brave.Wallet.UsageMonthly.2`_ **[D]** **[A]**

0. No
1. Yes

### Q38/39: How many days have you used the wallet in the past 7 days?
_`Brave.Wallet.UsageDaysInWeek`_ **[D]** **[A]**

0. No usage
1. One day
2. Two days
3. Three days
4. Four days
5. Five days
6. Six days
7. Seven days

### Q40: Did you switch search engines this week, and if so from what to what?
_`Brave.Search.SwitchEngine`_ **[D]** **[A]**

0. No
1. Yes, from Brave to Google
2. Yes, from Brave to DuckDuckGo
3. Yes, from Brave to Other
4. Yes, from Google to Brave
5. Yes, from DuckDuckGo to Brave
6. Yes, from Other to Brave
7. Yes, from Other to Other

### Q42: Has the Wallet keyring been created?
`Brave.Wallet.KeyringCreated` **[D]** **[A]**

0. No
1. Yes

### Q43: What is the DefaultWalletSetting?
`Brave.Wallet.DefaultWalletSetting` **[D]** **[A]**

0. AskDeprecated
1. None
2. CryptoWallets
3. BraveWalletPreferExtension
4. BraveWallet

### Q44: Was the IPFS local node installed? If so, is it still used?
`Brave.IPFS.LocalNodeRetention` **[D]** **[A]**

0. Never installed
1. Installed, in use
2. Installed, but disabled

### Q45: Are ads enabled? If not, were they enabled in the past? If so, for how long?
`Brave.Rewards.AdsEnabledDuration` **[D]** **[A]**

0. It was never on
1. It’s still on
2. Less than three hours
3. Less than three days
4. Less than three weeks
5. Less than three months
6. More than three months

### Q46: What is the global ad blocking shields setting?
_`Brave.Shields.AdBlockSetting`_ **[D]** **[A]**

0. Disabled
1. Standard
2. Aggressive

### Q47: What is the global fingerprinting shields setting?
_`Brave.Shields.FingerprintBlockSetting`_ **[D]** **[A]**

0. Disabled
1. Standard
2. Aggressive

### Q48: How many external feeds did you add last week?
_`Brave.Today.WeeklyAddedDirectFeedsCount`_ **[D]** **[A]**

0. None
1. 1 feed
2. 2 feeds
3. 3 feeds
4. 4 feeds
5. 5 feeds
6. 6 to 10 feeds
7. More than 10 feeds

### Q49: How many external feeds do you have in total?
_`Brave.Today.DirectFeedsTotal`_ **[D]** **[A]**

0. None
1. 1 feed
2. 2 feeds
3. 3 feeds
4. 4 feeds
5. 5 feeds
6. 6 to 10 feeds
7. More than 10 feeds

### Q50: How many Brave News cards did you view in the past week?
_`Brave.Today.WeeklyTotalCardViews`_ **[D]** **[A]**

0. None
1. 1
2. 2 to 10
3. 11 to 20
4. 21 to 40
5. 41 to 80
6. 81 to 100
7. 101 or more

### Q51: On how many domains has the user set the adblock setting to be lower (block less) than the default?
_`Brave.Shields.DomainAdsSettingsBelowGlobal`_ **[D]** **[A]**

0) 0
1) 1-5
2) 6-10
3) 11-20
4) 21-30
5) 31+

### Q52: On how many domains has the user set the adblock setting to be higher (block more) than the default?
_`Brave.Shields.DomainAdsSettingsAboveGlobal`_ **[D]** **[A]**

0) 0
1) 1-5
2) 6-10
3) 11-20
4) 21-30
5) 31+

### Q53: On how many domains has the user set the FP setting to be lower (block less) than the default?
_`Brave.Shields.DomainFingerprintSettingsBelowGlobal`_ **[D]** **[A]**

0) 0
1) 1-5
2) 6-10
3) 11-20
4) 21-30
5) 31+

### Q54: On how many domains has the user set the FP setting to be higher (block more) than the default?
_`Brave.Shields.DomainFingerprintSettingsAboveGlobal`_ **[D]** **[A]**

0) 0
1) 1-5
2) 6-10
3) 11-20
4) 21-30
5) 31+

### Q55: As an opted in Brave News user, when was the last time I used Brave News?
_`Brave.Today.LastUsageTime`_ **[D]** **[A]**

1) 0 - 6 days ago
2) 7 - 13 days ago
3) 14 - 20 days ago
4) 21 - 27 days ago
5) 28 - 59 days ago
6) 60 days ago or more

### Q56: As an opted in Brave News user, how many days did I use Brave News in the last 30 days?
_`Brave.Today.DaysInMonthUsedCount`_ **[D]** **[A]**

0) 0 days
1) 1 day
2) 2 days
3) 3 to 5 days
4) 6 to 10 days
5) 11 to 15 days
6) 16 to 20 days
7) More than 20 days

### Q57: As a first time user of Brave News this week, did I return again to use it during the first 7 day period?
_`Brave.Today.NewUserReturning`_ **[D]** **[A]**

0) I have never used Brave News
1) I have used Brave News, but I'm not a first time Brave News user this week
2) I'm a first time Brave News user this week but, no, I did not return the rest of the week
3) I'm a first time Brave News user this week and, yes, I returned and used it again the following day
4) I'm a first time Brave News user this week and, yes, I returned and used it again this week but not the following day

### Q58: Did the participant in Group A click on the promo and did they switch their default engine to Brave Search?
_`Brave.Search.Promo.Button`_ **[D]**

0) No, No
1) Yes, No
2) No, Yes
3) Yes, Yes

### Q59: Did the participant in Group B click on the promo and did they switch their default engine to Brave Search?
_`Brave.Search.Promo.Banner`_ **[D]**

0) No, No
1) Yes, No
2) No, Yes
3) Yes, Yes

### Q60. As a first time user of Brave Wallet this week, did I return again to use it during the first 7 day period?
_`Brave.Wallet.NewUserReturning`_ **[D]** **[A]**

0) I have never used Brave Wallet
1) I have used Brave Wallet, but I'm not a first time Brave Wallet user this week
2) I'm a first time Brave Wallet user this week but, no, I did not return the rest of the week
3) I'm a first time Brave Wallet user this week and, yes, I returned and used it again the following day
4) I'm a first time Brave Wallet user this week and, yes, I returned and used it again this week but not the following day

### Q61. If you accessed the Wallet page at least once, did you create/import a wallet?
_`Brave.Wallet.OnboardingConversion`_ **[D]** **[A]**

0) I accessed the wallet page, but did not set up the wallet
1) Yes, I have set up the wallet

### Q62. Did the participant in Group C click on the promo and did they switch their default engine to Brave Search?
_`Brave.Search.Promo.NewTabPage`_ **[D]** **[A]**

0) No, No
1) Yes, No
2) No, Yes
3) Yes, Yes

### Q63. As a Brave VPN user, when was the last time I enabled the Brave VPN?
_`Brave.VPN.LastUsageTime`_ **[D]** **[A]**

1) 0 - 6 days ago
2) 7 - 13 days ago
3) 14 - 20 days ago
4) 21 - 27 days ago
5) 28 - 59 days ago
6) 60 days ago or more

### Q64. As Brave VPN user, how many different days did I enable the Brave VPN in the last 30 days?
_`Brave.VPN.DaysInMonthUsed`_ **[D]** **[A]**

0) 0 days
1) 1 day
2) 2 days
3) 3 to 5 days
4) 6 to 10 days
5) 11 to 15 days
6) 16 to 20 days
7) More than 20 days

### Q65. As a first time user of the Brave VPN this week, did I enable it again the following day?
_`Brave.VPN.NewUserReturning`_ **[D]** **[A]**

0) I have never enabled the Brave VPN
1) I have enabled the Brave VPN, but I'm not a first time Brave VPN user this week
2) I'm a first time Brave VPN user this week but, no, I did not enable the Brave VPN the rest of the week
3) I'm a first time Brave VPN user this week and, yes, I did enable it again the following day
4) I'm a first time Brave VPN user this week and, yes, I enabled it again this week but not the following day

### Q66. Do you have Web Discovery Project enabled?
_`Brave.Search.WebDiscoveryEnabled`_ **[D]**

0) No
1) Yes

### Q67. What is the default Solana wallet setting?
_`Brave.Wallet.DefaultSolanaWalletSetting`_ **[D]** **[A]**

0. AskDeprecated
1. None
2. CryptoWallets
3. BraveWalletPreferExtension
4. BraveWallet

## Metrics Proposed/Under Development

Metrics in various states of development can be found in the `brave-browser` issue log with the `feature/new-metric` label:

https://github.com/brave/brave-browser/labels/feature%2Fnew-metric

## Removed metrics

### Q41: If you have turned off BR, how long did you have it on?
_`Brave.Rewards.EnabledDuration`_ **[D]** **[A]**

This question was removed in Brave v1.37, to be replaced by the better-defined `Brave.Rewards.AdsEnabledDuration` metric.

0. It was never on
1. It’s still on
2. 1 hour
3. 1 day
4. 1 week
5. 1 month
6. More than one month