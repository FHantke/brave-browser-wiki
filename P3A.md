# Privacy Preserving Product Analytics Metrics
This is a human-readable list of product analytics metrics collected.
The list of metrics collected can also be found [in the source
code](https://github.com/brave/brave-core/blob/26388acdc18107e9a43e862cc38ff6a637e68724/components/p3a/brave_p3a_service.cc#L51).
The analytics reported are not exact numbers, but are collected into aggregate
"buckets" or histograms. The process for this aggregation can also be verified [in
the source code](https://github.com/brave/brave-core/blob/cbfc3c2abceabf14e3528a558b7e0aa7378bb1c1/components/p3a/brave_histogram_rewrite.cc#L24).


## Current Metrics

### Q1: How long has this browser been open for the last seven days?
(_`Brave.Uptime.BrowserOpenMinutes`_)
1. N/A -- seven days have not passed since first open
2. 0 - 30min
3. 30min - 5hrs
4. 5hrs+

### Q2: Have you made Brave your default browser?
(_Brave.Core.IsDefault_) ~~(_`DefaultBrowser.State`_)~~
1. No
2. Yes

### Q3: Did you follow the on-boarding process
(_`Brave.Welcome.InteractionStatus`_)
1. Did not click within welcome screen
2. Clicked once 
3. Clicked twice or more but did not make it to the end
4. Made it to end of on-boarding flow

### Q4: Did you import settings and bookmarks, and if so from where?
(_`Brave.Importer.ImporterSource`_)
1. Did not import
2. Imported from Brave
3. Imported from Chrome
4. Imported from Firefox
5. Imported from Bookmarks HTML
6. Imported from Safari (answer only possible on macOS)

### Q5: How many bookmarks do you have?
(_`Brave.Core.BookmarksCountOnProfileLoad`_)
1. 0-5
2. 6-20
3. 21-100
4. Over 100

### Q6: How many open windows do you have?
(_`Brave.Core.WindowCount`_)
1. 0-1
2. 2-5
3. 5+

### Q7: How many open tabs do you have?
(_`Brave.Core.TabCount`_)
1. 0-1
2. 2-5
3. 6-10
4. 11-50
5. 50+

### Q8: What has been your interaction with the shields icon?
(_`Brave.Shields.UsageStatus`_)
1. Never clicked on shields icon
2. Clicked on it, but never made a change
3. Clicked on it, shut off shields for one or more sites
4. Clicked on it, changed Shields defaults or per-site shield settings in addition to (or in place of) shutting off shields on one or more sites

### Q9: Have you ever used a private window?
(_`Brave.Core.LastTimeIncognitoUsed`_)
1. Used in last 24h
2. Used in last week but not 24h
3. Used in last 28 days but not week
4. Ever used but not in last 28 days
5. Never used

### Q10: Have you ever used a Tor private window?
(_`Brave.Core.TorEverUsed`_)
1. Yes
2. No

### Q11: What is the state of your Brave Rewards wallet? [NOT IMPLEMENTED]
1. No wallet created
2. Wallet created; no grants claimed; no funds added
3. Wallet created; grant(s) claimed; no funds added
4. Wallet created: no grants claimed; funds added
5. Wallet created: grant(s) claimed; funds added
6. Wallet disabled after having been created (Brave Rewards switched off)

### Q12: How much BAT, excluding grants, is in your wallet?
(_`Brave.Rewards.WalletBalance.2`_)
1. No wallet created
2. Rewards Disabled
3. Less than 10 BAT (0-9), excluding grants
4. 10-50 BAT, excluding grants
5. Over 50 BAT, excluding grants~~

### Q13: Have you made use of Auto-contribute in Brave Rewards?
(_`Brave.Rewards.AutoContributionsState.2`_)
1. No wallet created
2. Rewards Disabled
3. Wallet created, Auto-Contribute off
4. Auto-contribute enabled, no contribution so far
5. Auto-contribute enabled, one successful contribution so far
6. Auto-contribute enabled, more than one successful contribution so far

### Q14: Have you made use of tips within Brave Rewards?
(_`Brave.Rewards.TipsState.2`_)
1. No wallet created
2. Rewards Disabled
3. Wallet created, no tips sent
4. Wallet created, one-time tip sent
5. Wallet created, at least one monthly tip queued up or sent
6. Wallet created, one-time sent and at least one monthly tip queued up or sent

### Q15: Have you enabled sync?
(_`Brave.Sync.Status`_)
1. No
2. Sync enabled with two devices
3. Sync enabled more than two devices

### Q16: How many extensions have you installed?
(_`Brave.Core.NumberOfExtensions`_ - only queried on program start)
1. None
2. One extension
3. Two to four extensions
4. Five or more extensions

### Q17: Have you enabled Brave Ads?
(_`Brave.Rewards.AdsState.2`_)
1. No wallet created
2. Rewards Disabled
3. Wallet created, ads not enabled
4. Wallet created and ads enabled 
5. Wallet created, ads enabled, then disabled, while Brave Rewards remains on
6. Wallet created, ads enabled, then disabled, Brave Rewards disabled

### Q18: How many questions the browser was able to answer within a week?
(_`Brave.P3A.SentAnswersCount`_)
1. None
2. Between 1 and 4
3. Between 5 and 9
4. 10 or more


## Metrics Proposed/Under Development
TBD