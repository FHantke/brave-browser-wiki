# Privacy Preserving Product Analytics Metrics
This is a human-readable list of product analytics metrics collected.
The list of metrics collected can also be found [in the source
code](https://github.com/brave/brave-core/blob/master/components/p3a/metric_names.h).
The analytics reported are not exact numbers, but are collected into aggregate
"buckets" or histograms. The process for this aggregation can also be verified [in
the source code](https://github.com/brave/brave-core/blob/cbfc3c2abceabf14e3528a558b7e0aa7378bb1c1/components/p3a/brave_histogram_rewrite.cc#L24).

General information about P3A can be found on [our support page](https://support.brave.com/hc/en-us/articles/9140465918093-What-is-P3A-in-Brave).

## Encoding Parameters

Metric reports encoded using the [STAR protocol](https://brave.com/privacy-updates/19-star/) use an [aggregation threshold](https://github.com/brave/brave-core/blob/master/components/p3a/constellation_helper.cc#L30) of 50 for each layer.

Submission of STAR-encoded reports is controlled by the `BraveP3AConstellation` feature flag [added in 1.52](https://github.com/brave/brave-core/pull/14399) and enabled by default [in 1.62](https://github.com/brave/brave-core/pull/20264). Parallel submission of metrics without STAR is suppressed by the `BraveP3ATypicalJSONDeprecation` feature flag for weekly-cadence questions and `BraveP3AOtherJSONDeprecation` for others, [added in 1.62](https://github.com/brave/brave-core/pull/20788).

## Current Metrics

**[D]** = supported on Desktop ([merged and enabled in 1.1.x](https://github.com/brave/brave-core/pull/3242))

**[A]** = supported on Android ([merged in 1.19.x](https://github.com/brave/brave-core/pull/7016) and [enabled in 1.20.x](https://github.com/brave/brave-core/pull/7550))

**[iOS]** = supported on iOS ([merged and enabled in 1.46.x](https://github.com/brave/brave-ios/pull/6308))

### Q1: How long has the browser been active for in the past day?
_`Brave.Uptime.BrowserOpenTime.2`_ **[D]** **[A]**

0. 30 minutes or less
1. 31 minutes - 1 hour
2. 1 - 2 hours
3. 2 - 3 hours
4. 3 - 5 hours
5. 5 - 7 hours
6. 7 - 10 hours
7. More than 10 hours

### Q2: Have you made Brave your default browser?
`_Brave.Core.IsDefault_` **[D]**
1. No
2. Yes

### Q3: What was the last screen that you viewed during the browser onboarding process?
_`Brave.Welcome.InteractionStatus.2`_ **[D]**

0. Only viewed the welcome screen, performed no action
1. Viewed the profile import screen
2. Viewed the diagnostic/analytics consent screen
3. Finished the onboarding process

### Q4: Did you import settings and bookmarks, and if so from where?
_`Brave.Importer.ImporterSource.2`_ **[D]**
1. Did not import
2. Imported from Brave
3. Imported from Chrome
4. Imported from Firefox
5. Imported from Bookmarks HTML
6. Imported from Safari (answer only possible on macOS)

### Q5: How many bookmarks do you have?
_`Brave.Core.BookmarkCount`_ **[D]** **[A]** **[iOS]**
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
_`Brave.Core.TabCount`_ **[D]** **[A]** **[iOS]**

0. 1
1. 2-5
2. 6-10
3. 11-50
4. 50+

### Q8: What has been your interaction with the shields icon?
_`Brave.Shields.UsageStatus`_ **[D]**

0. Never clicked on shields icon
1. Clicked on it, but never made a change
2. Clicked on it, shut off shields for one or more sites
3. Clicked on it, changed Shields defaults or per-site shield settings in addition to (or in place of) shutting off shields on one or more sites

### Q9: Have you ever used a private window?
_`Brave.Core.LastTimeIncognitoUsed`_ **[D]**
1. Used in last 24h
2. Used in last week but not 24h
3. Used in last 28 days but not week
4. Ever used but not in last 28 days
5. Never used

### Q10: Have you ever used a Tor private window?
_`Brave.Core.TorEverUsed`_ **[D]**

0. Yes
1. No

### Q12: How much BAT, excluding grants, is in your wallet?
_`Brave.Rewards.WalletBalance.3`_ **[D]** **[A]**
1. No wallet created
2. Wallet created, rewards disabled
4. Zero BAT, excluding grants
3. 1-9 BAT, excluding grants
4. 10-50 BAT, excluding grants
5. Over 50 BAT, excluding grants~~

### Q13: If you’re a Brave Rewards user with a custodial account linked, is your Auto-Contribute feature ON?
_`Brave.Rewards.AutoContributionsState.3`_ **[D]** **[A]**
0. No
1. Yes

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

### Q18: How many weekly questions did the browser send in the previous week?
_`Brave.P3A.SentAnswersCount`_ **[D]** **[A]**
1. None
2. Between 1 and 4
3. Between 5 and 9
4. 10 or more

### Q19: How many times did you search last week?
_`Brave.Omnibox.SearchCount.3`_ **[D]**
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
_`Brave.Search.DefaultEngine.4`_ **[D]** **[A]** **[iOS]**

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

`Brave.Savings.BandwidthSavingsMB` **[D]** **[A]** **[iOS]**

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

### Q25: In the past week, what was the highest amount of New Tab Pages created in a day?

`Brave.NTP.NewTabsCreated.3` **[D]** **[A]** **[iOS]**

0. 0 
1. 1
2. 2
3. 3
4. 4
5. 5-8
6. 9-15
7. 16+

### Q26: Is the sponsored new tab page option enabled?

`Brave.NTP.SponsoredMediaType` **[D]** **[A]** **[iOS]**

0. Disabled
1. Images only
2. Images and videos

### Q27: In the past week, on the day where you created the most tabs, how many of the New Tab Pages were sponsored?

`Brave.NTP.SponsoredNewTabsCreated.2` **[D]** **[A]** **[iOS]**

0. 0%
1. &gt; 0% and < 10%
2. 10% to < 20%
3. 20% to < 30%
4. 30% to < 40%
5. 40% to < 50%
6. 50% or more

### Q28: On the New Tab Page, did you click the Customize Settings icon?

`Brave.NTP.CustomizeUsageStatus.2` **[D]**

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

`Brave.Today.WeeklySessionCount` **[D]** **[iOS]**

0. Never
1. Once
2. 2-3 times
3. 4-7 times
4. 8-12 times
5. 13-18 times
6. 19-25 times
7. 26+ times

_* Refers to the issue whereby stats won't be reset or updated if the corresponding feature is not used in the next time period._

### Q36: Was reset sync progress marker procedure ever done?
_`Brave.Sync.ProgressTokenEverReset`_ **[D]** **[A]**
1. 0-Normal reset of progress marker due to 7th failure
2. 1-Reset progress marker due to 7th failure happening twice in a row in less than 30 minutes

### Q37: Have you used the wallet in this month?
_`Brave.Wallet.UsageMonthly`_ **[D]** **[A]**

1. Yes

### Q38: How you used the wallet this week?
_`Brave.Wallet.UsageWeekly`_ **[D]** **[A]**

1. Yes

### Q39: How you used the wallet in the past day?
_`Brave.Wallet.UsageDaily`_ **[D]** **[A]**

1. Yes

### Q40: Did you switch search engines this week, and if so from what to what?
_`Brave.Search.SwitchEngine`_ **[D]** **[A]** **[iOS]**

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

### Q44: Was the IPFS local node installed? If so, is it still used?
`Brave.IPFS.LocalNodeRetention` **[D]** **[A]**

0. Never installed
1. Installed, in use
2. Installed, but disabled

### Q45: If you have rewards enabled, which ad types do you have enabled?
`Brave.Rewards.AdTypesEnabled` **[D]** **[A]**

0. None
1. NTP only
2. Push only
3. NTP + Push

### Q46: What is the global ad blocking shields setting?
_`Brave.Shields.AdBlockSetting`_ **[D]** **[A]** **[iOS]**

0. Disabled
1. Standard
2. Aggressive

### Q47: What is the global fingerprinting shields setting?
_`Brave.Shields.FingerprintBlockSetting`_ **[D]** **[A]** **[iOS]**

0. Disabled
1. Standard
2. Aggressive

### Q48: How many external feeds did you add last week?
_`Brave.Today.WeeklyAddedDirectFeedsCount`_ **[D]** **[A]** **[iOS]**

0. None
1. 1 feed
2. 2 feeds
3. 3 feeds
4. 4 feeds
5. 5 feeds
6. 6 to 10 feeds
7. More than 10 feeds

### Q49: As a Brave News user this month, how many external feeds do you have in total?
_`Brave.Today.DirectFeedsTotal.3`_ **[D]** **[A]** **[iOS]**

0. 0
1. 1
2. 2-4
3. 5-8
4. 9-13
5. 14+

### Q50: How many Brave News cards did you view in the past week?
_`Brave.Today.WeeklyTotalCardViews`_ **[D]** **[A]** **[iOS]**

0. None
1. 1
2. 2 to 10
3. 11 to 20
4. 21 to 40
5. 41 to 80
6. 81 to 100
7. 101 or more

### Q51: On how many domains has the user set the adblock setting to be lower (block less) than the default?
_`Brave.Shields.DomainAdsSettingsBelowGlobal`_ **[D]** **[A]** **[iOS]**

0) 0
1) 1-5
2) 6-10
3) 11-20
4) 21-30
5) 31+

### Q52: On how many domains has the user set the adblock setting to be higher (block more) than the default?
_`Brave.Shields.DomainAdsSettingsAboveGlobal`_ **[D]** **[A]** **[iOS]**

0) 0
1) 1-5
2) 6-10
3) 11-20
4) 21-30
5) 31+

### Q53: On how many domains has the user set the FP setting to be lower (block less) than the default?
_`Brave.Shields.DomainFingerprintSettingsBelowGlobal`_ **[D]** **[A]** **[iOS]**

0) 0
1) 1-5
2) 6-10
3) 11-20
4) 21-30
5) 31+

### Q54: On how many domains has the user set the FP setting to be higher (block more) than the default?
_`Brave.Shields.DomainFingerprintSettingsAboveGlobal`_ **[D]** **[A]** **[iOS]**

0) 0
1) 1-5
2) 6-10
3) 11-20
4) 21-30
5) 31+

### Q55: As an opted in Brave News user, when was the last time I used Brave News?
_`Brave.Today.LastUsageTime`_ **[D]** **[A]** **[iOS]**

1) Within the last 6 days
2) 7 - 13 days ago
3) 14 - 20 days ago
4) 21 - 27 days ago
5) 28 - 59 days ago
6) 60 days ago or more

### Q56: As an opted in Brave News user, how many days did I use Brave News in the last 30 days?
_`Brave.Today.DaysInMonthUsedCount`_ **[D]** **[A]** **[iOS]**

0) 0 days
1) 1 day
2) 2 days
3) 3 to 5 days
4) 6 to 10 days
5) 11 to 15 days
6) 16 to 20 days
7) More than 20 days

### Q57: As a first time user of Brave News this week, did I return again to use it during the first 7 day period?
_`Brave.Today.NewUserReturning`_ **[D]** **[A]** **[iOS]**

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

### Q59: Did the participant in Group A/B/C/D click on the promo and did they switch their default engine to Brave Search?
_`Brave.Search.Promo.BannerA`_ _`Brave.Search.Promo.BannerB`_ _`Brave.Search.Promo.BannerC`_ _`Brave.Search.Promo.BannerD`_ `Brave.Search.Promo.DDGBannerA` `Brave.Search.Promo.DDGBannerB` `Brave.Search.Promo.DDGBannerC` `Brave.Search.Promo.DDGBannerD` **[D]**

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

### Q62. Did the participant in Group C click on the promo and did they switch their default engine to Brave Search?
_`Brave.Search.Promo.NewTabPage`_ **[D]** **[A]**

0) No, No
1) Yes, No
2) No, Yes
3) Yes, Yes

### Q63. As a Brave VPN user, when was the last time I enabled the Brave VPN?
_`Brave.VPN.LastUsageTime`_ **[D]** **[A]** **[iOS]**

1) Within the last 6 days
2) 7 - 13 days ago
3) 14 - 20 days ago
4) 21 - 27 days ago
5) 28 - 59 days ago
6) 60 days ago or more

### Q64. As Brave VPN user, how many different days did I enable the Brave VPN in the last 30 days?
_`Brave.VPN.DaysInMonthUsed`_ **[D]** **[A]** **[iOS]**

0) 0 days
1) 1 day
2) 2 days
3) 3 to 5 days
4) 6 to 10 days
5) 11 to 15 days
6) 16 to 20 days
7) More than 20 days

### Q65. As a first time user of the Brave VPN this week, did I enable it again the following day?
_`Brave.VPN.NewUserReturning`_ **[D]** **[A]** **[iOS]**

0) I have never enabled the Brave VPN
1) I have enabled the Brave VPN, but I'm not a first time Brave VPN user this week
2) I'm a first time Brave VPN user this week but, no, I did not enable the Brave VPN the rest of the week
3) I'm a first time Brave VPN user this week and, yes, I did enable it again the following day
4) I'm a first time Brave VPN user this week and, yes, I enabled it again this week but not the following day

### Q66. Do you have Web Discovery Project enabled?
_`Brave.Search.WebDiscoveryEnabled`_ **[D]**

0) No
1) Yes

### Q68. If you have viewed the cookie consent block prompt, how did you react?
_`Brave.Shields.CookieListPrompt`_ **[D]** **[A]** **[iOS]**

0. Have not seen the prompt
1. Have seen the prompt, but did not react
2. Clicked "No thanks" on the prompt
3. Clicked "Block cookie notices" on the prompt

### Q69. Do you have cookie consent notice blocking enabled?
_`Brave.Shields.CookieListEnabled`_ **[D]** **[A]** **[iOS]**

0. No
1. Yes

### Q70. What is your preferred Ethereum/Solana provider?
_`Brave.Wallet.EthProvider.4`_ and _`Brave.Wallet.SolProvider.2`_ **[D]**

0. None, because no wallet is setup
1. None, because the default provider setting is set to "Extensions (no fallback)"
2. Brave Wallet (wallet is setup, no installed extension; setting is "Extensions (Brave Wallet fallback)")
3. Brave Wallet (wallet is setup, an extension may potentially be installed but cannot override native provider; setting is "Brave Wallet")
4. Third-party extension (not overriding a setup Brave Wallet; setting is most likely "Extensions (Brave Wallet fallback)", but could be "Extensions (no fallback)")
5. Third-party extension (overriding a setup Brave Wallet; setting is "Extensions (Brave Wallet fallback)")

### Q71. As a user with Brave as the default search engine, how many pages did you load in the past week?
_`Brave.Core.PagesLoaded.BraveSearchDefault`_ **[D]** **[A]**

0. 0 pages
1. 1-10
2. 11-50
3. 51-100
4. 101-500
5. 501-1000
6. Greater than 1000 pages

### Q72. How many unique domains did you load in the past week?
_`Brave.Core.DomainsLoaded`_ **[D]** **[A]**

0. 0 domains
1. 1-4
2. 5-10
3. 11-30
4. 31-50
5. 51-100
6. Greater than 100 domains

### Q73. If you have ever sent a transaction, have you sent one in the past 7 days?
_`Brave.Wallet.EthTransactionSent`_, _`Brave.Wallet.SolTransactionSent`_, _`Brave.Wallet.BtcTransactionSent`_, _`Brave.Wallet.ZecTransactionSent`_ and _`Brave.Wallet.FilTransactionSent`_ [D] [A]

0) No
1) Yes

### Q74. As an active ETH/SOL/FIL/BTC/ZEC user (that currently has an active account, or previously has had one), how many accounts do you have with a non-zero balance?
_`Brave.Wallet.ActiveEthAccounts`_, _`Brave.Wallet.ActiveSolAccounts`_ and _`Brave.Wallet.ActiveFilAccounts`_, _`Brave.Wallet.ActiveBtcAccounts`_, _`Brave.Wallet.ActiveZecAccounts`_ [D] [A]

0) 0 accounts
1) 1 account
2) 2 accounts
3) 3 accounts
4) 4-7 accounts
5) 8+ accounts

### Q75. As a Brave Wallet user, when was the last time I unlocked the wallet?
_`Brave.Wallet.LastUsageTime`_ **[D]** **[A]**

1) Within the last 6 days
2) 7 - 13 days ago
3) 14 - 20 days ago
4) 21 - 27 days ago
5) 28 - 59 days ago
6) 60 days ago or more

### Q76. What menu functionality group do you use the most?
_`Brave.Toolbar.FrequentMenuGroup`_ **[D]**

0) Tab & window actions (new tab/new window)
1) Brave features (Wallet, Rewards, Sync)
2) Browser views (History, Bookmarks, Extensions, Settings)

### Q77. How often is the menu triggered and dismissed without an action taken in the past week?
_`Brave.Toolbar.MenuDismissRate`_ **[D]**

0) Menu was not opened in the past week
1) Less than 25% (exclusive) of opens
2) Between 25% (inclusive) and 50% (exclusive) of opens
3) Between 50% (inclusive) and 75% (exclusive) of opens
4) More than 75% of opens

### Q78. How many times was the menu opened in the past 7 days?
_`Brave.Toolbar.MenuOpens`_ **[D]** **[iOS]**

0) 0
1) 1-5
2) 6-15
3) 16-29
4) 30-49
5) 50+

### Q79. As a rewards or non-rewards user, how many News ads have I viewed in the past 7 days?
_`Brave.Today.RewardsAdViews`_ or _`Brave.Today.NonRewardsAdViews`_ **[D]** **[A]** **[iOS]**

0) 0
1) 1
2) 2-4
3) 5-12
4) 13-20
5) 21-40
6) 41-80
7) 81+

### Q80. How many days did you use the browser in the previous ISO week?
_`Brave.Core.WeeklyUsage`_ **[D]** **[A]** **[iOS]**

0) 0 days
1) 1 day
2) 2 days
3) 3 days
4) 4 days
5) 5 days
6) 6 days
7) 7 days

### Q81. If you enabled ads today, how many hours after browser installation did you enable ads?
_`Brave.Rewards.EnabledInstallationTime`_ **[D]** **[A]**

0) Less than an hour
1) 1 hour or more, less than 12 hours
2) 12 hours or more, less than 24 hours
3) 24 hours or more, less than 72 hours
4) 72 hours or more

### Q82. If you enabled rewards this week, how did you enable it?
_`Brave.Rewards.EnabledSource`_ **[D]**

0) Inline tip button
1) BAT icon in the address bar
2) Rewards card on NTP

### Q83. If rewards are disabled, did you click on the inline [Tip] button this week?
_`Brave.Rewards.InlineTipTrigger`_ **[D]**

1) Yes

### Q84. If rewards are disabled, did you click on the BAT icon in the URL this week?
_`Brave.Rewards.ToolbarButtonTrigger`_ **[D]**

1) Yes

### Q85. If you have Brave Rewards enabled with a custodial account connected, how many tips did you make in the past 30 days (monthly and one-time tips combined)? Monthly tips count so long as they are still in your monthly tips list, even if they do not go through.
_`Brave.Rewards.TipsSent.2`_ **[D]** **[A]**

0) 0 tips
1) 1 tip
2) 2-3 tips
3) 4+ tips

### Q86. If you are opted into sync, how many objects are synced?
_`Brave.Sync.SyncedObjectsCount.2`_ **[D]** **[A]**

0) 0 - 1000
1) 1001 - 10000
2) 10001 - 49000
3) 49001+

### Q87. If you are opted into sync, which types are enabled in Sync?
_`Brave.Sync.EnabledTypes`_ **[D]** **[A]**

0) No types or only Bookmarks
1) Only Bookmarks + History
2) More than (Bookmarks + History) but less than (Sync All)
3) All types

### Q88. As a user that used Playlist at least once, when was the last time I used or added content to Brave Playlist?
_`Brave.Playlist.LastUsageTime`_ **[D]** **[A]**

1) 0 - 6 days ago (less than a week)
2) 7 - 13 days ago (one week ago or more)
3) 14 - 20 days ago (two weeks ago or more)
4) 21 - 27 days ago (three weeks ago or more)
5) 28 - 59 days ago (four weeks ago or more)
6) 60 days ago or more (two months ago or more)

### Q89. As a first-time user of Brave Playlist this week, did I return again to use or add content to Brave Playlist during the first 7-day period?
_`Brave.Playlist.NewUserReturning`_ **[D]** **[A]**

1) I have used Brave Playlist, but I'm not a first-time Brave Playlist user this week
2) I'm a first-time Brave Playlist user this week but, no, I did not return the rest of the week
3) I'm a first-time Brave Playlist user this week and, yes, I returned and used it again the following day
4) I'm a first-time Brave Playlist user this week and, yes, I returned and used it again this week but not the following day

### Q90. As a user that used Playlist this week, how many days did I use or add content to Brave Playlist in the past 7 days?
_`Brave.Playlist.UsageDaysInWeek`_ **[D]** **[A]**

1) 1 to 2 days
2) 3 to 4 days
3) 5 to 6 days
4) 7 days

### Q91. As a new user of Playlist this week, how long did it take for new Brave users to use Playlist for the first time?
_`Brave.Playlist.FirstTimeOffset`_ **[D]** **[A]**

0) Less than 1 day
1) 1 - 6 days
2) 7 - 13 days 
3) 14 - 20 days 
4) 21 - 27 days 
5) Longer than 27 days

### Q92. How many profiles do you have?
_`Brave.Core.ProfileCount`_ **[D]**

1) 1
2) 2
3) 3
4) 4-5
5) 6+

### Q93. If you changed your News opt-in status today, what did you change it to?
_`Brave.Today.IsEnabled`_ **[D]** **[A]**

0) Disabled
1) Enabled

### Q94. Did you use Brave News this month?
_`Brave.Today.UsageMonthly`_ **[D]** **[A]** **[iOS]**

1) Yes

### Q95. Did you use Brave News in the past day?
_`Brave.Today.UsageDaily`_ **[D]** **[A]** **[iOS]**

1) Yes

### Q96. Did you use the browser this month?
_`Brave.Core.UsageMonthly`_ **[D]** **[A]** **[iOS]**

1) Yes

### Q97. Did you use the browser in the past day?
_`Brave.Core.UsageDaily`_ **[D]** **[A]** **[iOS]**

1) Yes

### Q98. If you accessed the Wallet onboarding page this week, where did you stop in the onboarding process?
_`Brave.Wallet.OnboardingConversion.3`_ **[D]** **[A]**

0) Viewed onboarding splash
1) Between "Legal stuff" and "Create password"
2) During "recovery phrase" steps
3) Completed onboarding after completing "recovery phrase" steps
4) Completed onboarding after skipping "recovery phrase" steps

### Q99. If you created a wallet within the last week, did you make a deposit this week?
_`Brave.Wallet.NewUserBalance`_ **[D]** **[A]**

1) Yes

### Q100: Do you have iOS display zoom enabled?
_`Brave.Accessibility.DisplayZoomEnabled`_ **[iOS]**

0. No
1. Yes

### Q101: Which location Bottom bar being used?
_`Brave.General.BottomBarLocation`_ **[iOS]**

0. Top
1. Bottom

### Q102: How many times did you use reader mode in the last 7 days?
_`Brave.ReaderMode.NumberReaderModeActivated`_ **[iOS]**

0. 0
1. 1-5
2. 6-20
3. 21-50
4. 51+

### Q103: What is the document directory size in MB?
_`Brave.Core.DocumentsDirectorySizeMB`_ **[iOS]**
0. 0-50
1. 50-200
2. 200-500
3. 500-1000
4. 1000+

### Q104: Do you have privacy report enabled?
_`Brave.PrivacyHub.IsEnabled`_ **[A]**

1. Yes

### Q105: If you opened privacy report in the past 30 days, how many times did you open it?
_`Brave.PrivacyHub.Views`_ **[A]**

0) 1
1) 2-10
2) 11-20
3) 21+

### Q106: As a user with vertical tabs enabled, what is the max amount of open tabs this week?
_`Brave.VerticalTabs.OpenTabs`_ **[D]**

0. 1
1. 2-5
2. 6-10
3. 11-50
4. 51+

### Q107: As a user with vertical tabs enabled and had at least one pinned tab this week, what is the max amount of pinned tabs this week?
_`Brave.VerticalTabs.PinnedTabs`_ **[D]**

0. 1-2
1. 3-5
2. 6+

### Q108: As a user with vertical tabs mode enabled and had at least one tab group this week, what is the max amount of tab groups this week?
_`Brave.VerticalTabs.GroupTabs`_ **[D]**

0. 1-2
1. 3-5
2. 6+

### Q109: Do you have Leo enabled?
_`Brave.AIChat.Enabled.2`_ **[D]** **[A]**

1. Yes, as a free user
2. Yes, as a premium user

### Q110: Did you use Leo in the past day?
_`Brave.AIChat.UsageDaily.2`_ **[D]** **[A]**

1. Yes, as a free user
2. Yes, as a premium user

### Q111: If you used Leo in the past week, how many chats did you engage in during the period?
_`Brave.AIChat.ChatCount`_ **[D]** **[A]**

0. 1
1. 2 to 5
2. 6 to 10
3. 11 to 20
4. 21 to 50
5. More than 50

### Q112: If you used Leo in the past week, what was the average amount of prompts per conversation during the period?
_`Brave.AIChat.AvgPromptCount`_ **[D]** **[A]**

0. 1 to 2
1. 3 to 5
2. 6 to 10
3. 11 to 20
4. More than 20

### Q113: If you viewed the NFT gallery this week, how many NFTs are displayed in the gallery?
_`Brave.Wallet.NFTCount`_ **[D]** **[A]**

0. 0
1. 1 - 4
2. 5 - 20
3. More than 20

### Q114: Did you use the NFT gallery for the first time this week?
_`Brave.Wallet.NFTNewUser`_ **[D]** **[A]**

1. Yes

### Q115: As a user that has used the wallet at least once, do you have NFT discovery enabled?
_`Brave.Wallet.NFTDiscoveryEnabled`_ **[D]** **[A]**

0. No
1. Yes

### Q116: Do you have the Sidebar setting set to "Always" or "On mouseover"?
_`Brave.Sidebar.Enabled`_ **[D]**

1. Yes

### Q117: As a phone user, in the past 7 days, what % of your navigation button actions consist of forward navigation?
_`Brave.Toolbar.ForwardNavigationAction`_ **[iOS]**

0. 0-1%
1. 1-3%
2. 3-5%
3. 5-10%
4. 10-20%
5. 20-100%

### Q118: If you switched search engines this week from Brave Search to something else, how many queries did you do before switching? (via address bar only)
_`Brave.Search.QueriesBeforeChurn`_ **[D]** **[A]**

0. 0
1. 1
2. 2
3. 3-5
4. 6-10
5. 11-20
6. 21-40
7. More than 40

### Q119: If you have secure DNS enabled, what setting are you using?
_`Brave.DNS.SecureSetting`_ **[D]** **[A]**

1. Current service provider
2. Predefined / custom provider

### Q120: If your secure DNS setting is set to "current service provider" and at least one DNS request was auto-upgraded in the past week, what percent of queries were made over DoH in the past week?"
_`Brave.DNS.AutoSecureRequests.2`_, _`Brave.DNS.AutoSecureRequests.Quad9.2`_, _`Brave.DNS.AutoSecureRequests.Wikimedia.2`_ or _`Brave.DNS.AutoSecureRequests.Cloudflare.2`_ **[D]** **[A]**

0. 0%
1. Greater than 0%, less than 5%
2. Greater than 5%, less than 50%
3. Greater than 50%, less than 90%
4. Greater than 90%

### Q121: If you are an opted-in Leo user, what % of your Omnibox selections were the Ask Leo option?
_`Brave.AIChat.OmniboxOpens`_ **[D]**

0. None
1. Less than 1%
2. Between 1% and 3%
3. Between 3% and 5%
4. Between 5% and 10%
5. Between 10% and 25%
6. More than 25%

### Q122: If you are an opted-in Leo user, and you used the Leo omnibox option in the previous week, did you select the Leo omnibox option proportionally more this week vs last week?
_`Brave.AIChat.OmniboxWeekCompare`_ **[D]**

0. More last week
1. More this week

### Q123: As a first time Leo user, what was your entry point?
_`Brave.AIChat.AcquisitionSource`_ **[D]**

0. Omnibox option
1. Sidebar
2. Context menu
3. Toolbar button
4. App menu item
5. Omnibox command

### Q124: Do you have Web Discovery Project & notification ads enabled?
_`Brave.Search.WebDiscoveryAndAds`_ **[D]**

0. No
1. Yes

### Q125: Do you have a popular third-party adblock extension installed?
_`Brave.Extensions.AdBlock`_ **[D]**

0. No
1. Yes

### Q126: In the past week, how often are address bar entries/queries performed in new tabs compared to existing tabs?
_`Brave.Core.LocationNewEntries`_ **[A]** **[iOS]**

0. Less than 25% of total address bar entries performed in new tabs
1. 25% -50% of total address bar entries performed in new tabs
2. 50% -75% of total address bar entries performed in new tabs
3. More than 75% of total address bar entries performed in new tabs

### Q127: In the past week, when creating new tabs, how often do you use the [+] button compared to the tab switcher button?
_`Brave.Core.NewTabMethods`_ **[A]** **[iOS]**

0. Less than 25% of total new tabs created using [+] button
1. 25% -50% of total new tabs created using [+] button
2. 50% -75% of total new tabs created using [+] button
3. More than 75% of total new tabs created using [+] button

### Q128: If you triggered the Rewards panel as a non-rewards user or a new Rewards user this week, did you enable Rewards?
_`Brave.Rewards.MobileConversion`_ **[A]**

0. No
1. Yes

### Q129: If you triggered the Rewards panel at least once as a Rewards user in the last 7 days, how many times did you trigger the panel?
_`Brave.Rewards.MobilePanelCount`_ **[A]**

0. 1-5
1. 6-10
2. 11-50
3. 51+

### Q130: Did you view the WebTorrent page at least once this week?
_`Brave.WebTorrent.UsageWeekly`_ **[D]**

1. Yes

### Q131: How many times have you been presented the Request-OTR interstitial in the past month?
_`Brave.RequestOTR.InterstitialShown`_ **[D]**

0) 0
1) 1
2) 2
3) 3-5
4) 6-10
5) 11+

### Q132: How many times has the user opted into Request-OTR mode on a site in the past month?
_`Brave.RequestOTR.SessionCount`_ **[D]**

0) 0
1) 1
2) 2
3) 3-5
4) 6-10
5) 11+

### Q133: If Request-OTR interstitial was shown at least once in the past month, how long did the user view the page, on average?
_`Brave.RequestOTR.InterstitialDuration`_ **[D]**

0) Less than or equal to 5 seconds
1) Greater than 5, but less than or equal to 10 seconds
2) Greater than 10, but less than or equal to 15 seconds
3) Greater than 15, but less than or equal to 30 seconds
4) Greater than 30, but less than or equal to 60 seconds
5) Greater than 60 sec

### Q134: Did you use Leo this month?
_`Brave.AIChat.UsageMonthly`_ **[D]** **[A]**

1. Yes, as a free user
2. Yes, as a premium user

### Q135: Did you use Leo this week?
_`Brave.AIChat.UsageWeekly`_ **[D]** **[A]**

1. Yes, as a free user
2. Yes, as a premium user

### Q136: As a first time user of Brave Leo this week, did I return again to use it during the first 7 day period?
_`Brave.AIChat.NewUserReturning`_ **[D]** **[A]**

0. I have never used the feature
1. I have used the feature, but I'm not a first time feature user this week
2. I'm a first time feature user this week but, no, I did not return the rest of the week
3. I'm a first time feature this week and, yes, I returned and used it again the following day
4. I'm a first time feature user this week and, yes, I returned and used it again this week but not the following day

### Q137: As an opted in Brave Leo user, when was the last time I used Brave Leo? (reported monthly)
_`Brave.AIChat.LastUsageTime`_ **[D]** **[A]**

1. Within the last 6 days
2. 7 - 13 days ago
3. 14 - 20 days ago
4. 21 - 30 days ago

### Q138: If you are Rewards user who opened the Rewards panel in the past week, how many times did you open it?
_`Brave.Rewards.DesktopPanelCount`_ **[D]**

0. 1-5
1. 6-10
2. 11-50
3. 50+

### Q139: If you are Rewards user who opened the Rewards page in the past month, how many times did you open it?
_`Brave.Rewards.PageViewCount`_ **[D]** **[A]**

0. 1-2
1. 3-5
2. 6-10
3. 11-50
4. 51+

### Q140: What is your primary language? (reported via STAR/Constellation only)
_`Brave.Core.PrimaryLang`_ **[D]** **[A]** **[iOS]**

See https://github.com/brave/brave-core/blob/master/components/misc_metrics/language_metrics.cc#L26-L210 for an ordered list of answers.

### Q141: Do you have a payment method saved in your autofill settings?
`Brave.Autofill.PaymentMethodPresent` **[D]** **[A]**

0. No
1. Yes

### Q142: How many pages did you reload in the past week?
`Brave.Core.PagesReloaded` **[D]** **[A]**

0. 0 pages
1. 1-10
2. 11-50
3. 51-100
4. 101-500
5. 501-1000
6. Greater than 1000 pages

### Q143: If you have a custom position configured for ads notifications, what area is it in?
_`Brave.Rewards.CustomNotificationAdPosition`_  **[D]**

1. Top left
2. Top center
3. Top right
4. Center left
5. Center right
6. Bottom left
7. Bottom center
8. Bottom right

### Q144: Is it likely that you have made Brave the default browser on iOS?
_`Brave.IOS.IsLikelyDefault`_ **[iOS]**

0. Not enough information
1. No
2. Yes

### Q145: If you have used Brave News in the last week and clicked on at least one card, how many cards did you click on? (reported weekly, desktop & Android)
_`Brave.Today.WeeklyTotalCardClicks`_ **[D]** **[A]**

0. 1-2
1. 3-5
2. 6 -10
3. 11-15
4. 16-20
5. 21-25
6. 26+

### Q146: If you have clicked on 5 or more cards in the last week, what is the average rank (depth) of the cards you clicked on in the For You/Following feeds? (reported weekly, desktop & Android)
_`Brave.Today.ClickCardDepth`_ **[D]** **[A]**

0. 1-3 cards deep
1. 4 to 6 cards deep
2. 7 to 10 cards deep
3. 11 to 15 cards deep
4. 16 to 20 cards deep
5. 21+

### Q147: If you are a monthly active News user, how many channels are you subscribed to? (reported monthly, desktop & Android only)
_`Brave.Today.ChannelCount.2`_ **[D]** **[A]**

0. 0
1. 1
2. 2-4
3. 5-8
4. 9-13
5. 14+

### Q148: If you are a monthly active News user, how many publishers are you subscribed to (excluding direct feeds)? (reported monthly, desktop & Android only)
_`Brave.Today.PublisherCount.2`_ **[D]** **[A]**

0. 0
1. 1
2. 2-4
3. 5-8
4. 9-13
5. 14+

### Q149: If you have used Brave News in the last week and you used the sidebar filter at least once, how many sessions included the usage of the sidebar filters? (reported weekly, desktop only)
_`Brave.Today.SidebarFilterUsages`_ **[D]** **[A]**

0. 1
1. 2-4
2. 5-7
3. 8-10
4. 11+

### Q150: If you filed a web compatibility report this week, how did you file it?

_`Brave.Webcompat.UISource`_ **[D]** **[A]**

0. Via the Shields panel
1. Via the app menu (desktop only)

### Q151: If your "upgrade connections to HTTPS" setting is set to "standard" or "strict", how many page requests failed the auto-upgrade to HTTPS this week?
_`Brave.Core.FailedHTTPSUpgrades.2`_  **[D]** **[A]**

0. 0%
1. Above 0%, less than or equal to 0.25%
2. Above 0.25%, less than or equal to 0.5%
3. Above 0.5, less than or equal to 1%
4. Above 1%, less than or equal to 3%
5. Above 3%, less than or equal to 7%
6. Greater than 7%

### Q152: As a free tier user opted in to Leo, how many times did you use right click actions in the past week?
_`Brave.AIChat.ContextMenu.FreeUsages` and `Brave.AIChat.ContextMenu.PremiumUsages`_  **[D]**

0. 0
1. 1
2. 2
3. 3 to 5
4. 6 to 10
5. 11 to 20
6. 21 to 50
7. More than 50

### Q153: As a user of Leo Right-click Actions this week, what is the most used action category?
_`Brave.AIChat.ContextMenu.MostUsedAction`_  **[D]**

0. Summarize
1. Explain
2. Paraphrase
3. Create Tagline
4. Create Social Media Post
5. Improve
6. Change Tone
7. Change Length

### Q154: As an opted in Leo user, when was the last time you used the Right-click Actions feature?
_`Brave.AIChat.ContextMenu.LastUsageTime`_  **[D]**

1. Within the last 6 days
2. 7 - 13 days ago
3. 14 - 20 days ago
4. 21 - 30 days ago

### Q155: If you are included in the day zero experiment, how long ago did you install the browser?
_`Brave.DayZero.A.InstallTime`_ or _`Brave.DayZero.B.InstallTime`_  **[D]** **[A]**

Answers range from 0 to 30 days.

### Q156: As a "sidebar enabled" experiment participant, do you have Leo enabled?
`Brave.AIChat.Enabled.SidebarEnabledA` or `Brave.AIChat.Enabled.SidebarEnabledB` **[D]**

1. Yes, as a free user
2. Yes, as a premium user

### Q157: As a "sidebar enabled" experiment participant, did you use Leo in the past day?
`Brave.AIChat.UsageDaily.SidebarEnabledA` or `Brave.AIChat.UsageDaily.SidebarEnabledB` **[D]**

1. Yes, as a free user
2. Yes, as a premium user

### Q158: As a "sidebar enabled" experiment participant, did you use Leo in the past week?
`Brave.AIChat.UsageWeekly.SidebarEnabledA` or `Brave.AIChat.UsageWeekly.SidebarEnabledB` **[D]**

1. Yes, as a free user
2. Yes, as a premium user

### Q159: As a "sidebar enabled" experiment participant, did you change the Sidebar settings?
`Brave.Sidebar.SettingChange.SidebarEnabledA` or `Brave.Sidebar.SettingChange.SidebarEnabledB` **[D]**

0. No
1. Yes, I changed Show Sidebar to "On mouse over"
2. Yes, I changed Show Sidebar to "Never"

### Q160: Did you temporarily allow/block individual scripts this week?
`Brave.Shields.AllowScriptOnce` **[D]**

1. Yes

### Q161: Which extra ad-block filter lists do you have enabled this month?
`Brave.Shields.FilterLists` **[D]** **[A]**

0. None
1. One or more hardcoded filter lists
2. One or more custom filter lists
3. Both hardcoded & custom filter lists

### Q162: Do you have the "forget me when I close this site" setting enabled?
`Brave.Shields.ForgetFirstParty` **[D]** **[A]**

0. Not globally, and not per-site
1. Globally enabled, no exceptions for any sites
2. Globally enabled, but the setting is disabled for at least one site
3. Not globally, but the setting is enabled for at least one site

### Q163: If you’re a Rewards user with a custodial account connected, do you have one or more monthly contributions configured?
`Brave.Rewards.RecurringTip` **[D]** **[A]**

0. No
1. Yes

### Q164: Did you make a Brave Search query today?
`Brave.Search.BraveDaily` **[D]** **[A]**

1. Yes

### Q165: If you have rewards enabled, which ad types do you have enabled?
`Brave.Rewards.AdTypesEnabled.2` **[D]** **[A]**

0. None
1. NTP only
2. Push only
3. NTP + Push
4. Search ads only
5. Search + NTP
6. Search + Push
7. Search + NTP + Push

### Q166: If you have Rewards enabled and a payout account connected, did you enable search ads this week?
`Brave.Rewards.SearchResultAdsOptin` **[D]**

1. Yes

### Q167: If you are a Rewards user, did you view the 30-day Ads history page in the past week? 
`Brave.Rewards.AdsHistoryView` **[D]**

1. Yes

### Q168: If you used the search widget in the past 7 days, how many queries did you perform this week?
`Brave.Search.WidgetUsage` **[D]**

0. 1-10
1. 11-30
2. 31-40
3. 41+

### Q169: If the search widget is enabled, what is your default engine for the search widget?
`Brave.Search.WidgetDefault` **[D]**

0. Brave
1. Google
2. DDG
3. Qwant
4. Bing
5. Startpage
6. Ecosia
7. Other

### Q170: Did you delete your Brave Ads Data this week?
`Brave.Ads.ClearData` **[D]** **[A]** **[iOS]**

1. Yes

### Q171: What color mode are you using?
`Brave.Theme.BrowserColorScheme` **[D]**

0. OS default
1. Dark
2. Light

### Q172: Are you using the default theme colors?
`Brave.Theme.ThemeColorDefault` **[D]**

0. No
1. Yes

### Q173: Do you have "Show search suggestions" enabled?
`Brave.Search.SearchSuggest` **[D]**

0. No
1. Yes

### Q174: Did you make a Google query using the NTP search widget this week?
`Brave.Search.GoogleWidgetUsage` **[D]**

1. Yes

### Q175: If you are an opted-in Leo user and used Leo in the past 7 days, what was the most used entry point in the past 7 days?
`Brave.AIChat.MostUsedEntryPoint` **[D]**

0. Omnibox item
1. Sidebar
2. Context menu
3. Toolbar button
4. App menu item
5. Omnibox command

### Q176: If you are a first-time user this week and you viewed at least one page, how long after the install time did you view your first web page?
`Brave.Core.FirstPageLoadTime` **[D]** **[A]**

0. 0-5 minutes
1. 6-10 minutes
2. 11 minutes - 1 hour
3. 1 hour - 4 hours
4. 4 hours - 1 day
5. Greater than a day

### Q177. As a user with a non-Brave default search engine, how many pages did you load in the past week?
_`Brave.Core.PagesLoaded.OtherSearchDefault`_ **[D]** **[A]**

0. 0 pages
1. 1-10
2. 11-50
3. 51-100
4. 101-500
5. 501-1000
6. Greater than 1000 pages

## New Tab Page Ad Metrics

Metrics are reported for New Tab Page ad interactions, for those who are not opted into Brave Rewards. Three event types are reported for each ad campaign, on a daily basis:

- `views`: amount of views for an ad in the past day
- `clicks`: amount of clicks for an ad in the past day
- `lands`: amount of times a user clicked on the ad, and stayed on the subsequent page for 10 or more seconds

A metric name is constructed for each ad campaign and event type: `creativeInstanceId.<ad campaign id>.<event type>`

An additional metric is reported for the total amount of ad campaigns that the user interacted within the past day (`creativeInstanceId.total.count`).

The bucket values for all of the above metrics are as follows:

0. 0 interactions
1. 1 interaction
2. 2 interactions
3. 3 interactions
4. 4 -8 interactions
5. 9-12 interactions
6. 13-16 interactions
7. 17+ interactions

## Metrics Proposed/Under Development

Metrics in various states of development can be found in the `brave-browser` issue log with the `feature/new-metric` label:

https://github.com/brave/brave-browser/labels/feature%2Fnew-metric