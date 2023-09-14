# Privacy Preserving Product Analytics Metrics
This is a human-readable list of product analytics metrics collected.
The list of metrics collected can also be found [in the source
code](https://github.com/brave/brave-core/blob/master/components/p3a/metric_names.h).
The analytics reported are not exact numbers, but are collected into aggregate
"buckets" or histograms. The process for this aggregation can also be verified [in
the source code](https://github.com/brave/brave-core/blob/cbfc3c2abceabf14e3528a558b7e0aa7378bb1c1/components/p3a/brave_histogram_rewrite.cc#L24).


## Current Metrics

**[D]** = supported on Desktop ([merged and enabled in 1.1.x](https://github.com/brave/brave-core/pull/3242))

**[A]** = supported on Android ([merged in 1.19.x](https://github.com/brave/brave-core/pull/7016) and [enabled in 1.20.x](https://github.com/brave/brave-core/pull/7550))

**[iOS]** = supported on iOS ([merged and enabled in 1.46.x](https://github.com/brave/brave-ios/pull/6308))

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
_`Brave.Core.BookmarksCountOnProfileLoad.2`_ **[D]** **[A]** **[iOS]**
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

`Brave.NTP.NewTabsCreated.2` **[D]** **[A]** **[iOS]**

0. 0 
1. 1
2. 2
3. 3
4. 4
5. 5-8
6. 9-15
7. 16+

### Q26: Is the sponsored new tab page option enabled?

`Brave.NTP.SponsoredImagesEnabled` **[D]** **[A]** **[iOS]**

0. Disabled
1. Enabled

### Q27: In the past week, on the day where you created the most tabs, how many of the New Tab Pages were sponsored?

`Brave.NTP.SponsoredNewTabsCreated` **[D]** **[A]** **[iOS]**

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

### Q49: How many external feeds do you have in total?
_`Brave.Today.DirectFeedsTotal`_ **[D]** **[A]** **[iOS]**

0. None
1. 1 feed
2. 2 feeds
3. 3 feeds
4. 4 feeds
5. 5 feeds
6. 6 to 10 feeds
7. More than 10 feeds

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
_`Brave.Search.Promo.BannerA`_ _`Brave.Search.Promo.BannerB`_ _`Brave.Search.Promo.BannerC`_ _`Brave.Search.Promo.BannerD`_ **[D]**

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
2. Brave Wallet (wallet is setup, no installed extension)
3. Brave Wallet (wallet is setup, an extension may potentially be installed but cannot override native provider)
4. Third-party extension (not overriding a setup Brave Wallet)
5. Third-party extension (overriding a setup Brave Wallet)

### Q71. How many pages did you load in the past week?
_`Brave.Core.PagesLoaded`_ **[D]** **[A]** **[iOS]**

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
_`Brave.Wallet.EthTransactionSent`_, _`Brave.Wallet.SolTransactionSent`_ and _`Brave.Wallet.FilTransactionSent`_ [D] [A]

0) No
1) Yes

### Q74. As an active ETH/SOL/FIL user (that currently has an active account, or previously has had one), how many accounts do you have with a non-zero balance?
_`Brave.Wallet.ActiveEthAccounts`_, _`Brave.Wallet.ActiveSolAccounts`_ and _`Brave.Wallet.ActiveFilAccounts`_ [D] [A]

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

### Q79. How many News ads have I viewed in the past 7 days?
_`Brave.Today.WeeklyDisplayAdsViewedCount`_ **[D]** **[A]** **[iOS]**

0) 0
1) 1
2) 2-4
3) 5-12
4) 13-20
5) 21-40
6) 41-80
7) 81+

### Q80. How many days did you use the browser in the previous ISO week?
_`Brave.Core.WeeklyUsage`_ **[D]** **[A]**

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
_`Brave.Sync.SyncedObjectsCount`_ **[D]** **[A]**

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
_`Brave.Today.UsageMonthly`_ **[D]** **[A]**

1) Yes

### Q95. Did you use Brave News in the past day?
_`Brave.Today.UsageDaily`_ **[D]** **[A]**

1) Yes

### Q96. Did you use the browser this month?
_`Brave.Core.UsageMonthly`_ **[D]** **[A]**

1) Yes

### Q97. Did you use the browser in the past day?
_`Brave.Core.UsageDaily`_ **[D]** **[A]**

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
1. Yes
2. No

### Q101: Which location Bottom bar being used?
_`Brave.General.BottomBarLocation`_ **[iOS]**
1. Top
2. Bottom

### Q102: How many times did you use reader mode in the last 7 days?
_`Brave.ReaderMode.NumberReaderModeActivated`_ **[iOS]**
1. 0
2. 1-5
3. 6-20
4. 21-50
5. 51+

### Q103: What is the document directory size in MB?
_`Brave.Core.DocumentsDirectorySizeMB`_ **[iOS]**
1. 0-59
2. 50-200
3. 200-500
4. 500-1000
5. 1000+

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
_`Brave.AIChat.Enabled`_ **[D]** **[A]**

1. Yes

### Q110: Did you use Leo in the past day?
_`Brave.AIChat.UsageDaily`_ **[D]** **[A]**

1. Yes

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

## Metrics Proposed/Under Development

Metrics in various states of development can be found in the `brave-browser` issue log with the `feature/new-metric` label:

https://github.com/brave/brave-browser/labels/feature%2Fnew-metric