## What are referral codes?

Referral codes, or ref codes for short, are 6-character alphanumeric codes used in Brave on all platforms, including mobile, to identify where the build was downloaded from.

* On Windows and macOS, these codes are either hard-coded in the build or embedded in the binary’s filename and then extracted from the filename on first run.
* On iOS, when a user visits a referral page and chooses to visit the App Store to install Brave, their IP address is hashed and temporarily saved with the 6 digit referral code. When the user launches Brave on iOS for the first time, the app makes a request and matches their IP address hashes to identify the referral code they used to install Brave. At this time, the IP hash entry is permanently deleted.
* On Android, when a user visits a referral page, the website redirects to Google Play with the corresponding ref code. When a user launches Brave on Android for the first time, we retrieve the corresponding ref code from the source of installation.

Referral codes are not personally identifiable and are not unique to a user unless they are the only person who has downloaded Brave from a particular source. The codes are saved in browser state and may be included in pings for up to 90 days after install, at which point they are deleted.

## Why does Brave use referral codes?

* Referral codes are used to identify builds downloaded via partnerships or paid ads that Brave is running. We use this data to measure product retention for these partnerships/ads.
* Referral codes are used for brave.com desktop downloads to understand the retention of specific acquisition channels. These referral codes are to also remove bots from influencing product metrics. Examples of the ref codes used on brave.com include:
   - BRV010: Google Search downloads
   - BRV011: Bing Search downloads
   - BRV012: Yahoo Search downloads
   - BRV013: Brave Search downloads
   - BRV029: Downloads from all other search engines
   - BRV002: Downloads from direct traffic to brave.com
* Referral codes are used for the now retired Brave [creators referral program](https://creators.brave.com/) to measure how many installs of Brave a creator has driven through their referral program. This program was shutdown on February 2023.

## What data is sent with the referral code in pings?

Clients send their platform and the referral code in the initial referral code ping, except on iOS, where the initial request doesn’t contain the referral code but instead the referral code is inferred based on an IP hash. Based on their IP, we infer a geographic region for them. The client gets a unique `download_id` in response to the initial ping. In the 30-day confirmation ping, they also send the `download_id`. After the confirmation ping, `download_id` is deleted from browser state. After 90 days, the refcode is deleted from browser state.

The Windows stub installer sends 3 additional events, `['startup', 'download-complete', 'installer-run']`, for pings that are called when the stub installer starts, when the stub installer successfully downloads the full installer, and when the stub installer successfully runs the full installer. These are mainly used to help us debug potential problems in the installer flow.

These pings are sent over TLS and do not include cookies.

The diagram below shows the full flow for non-stub installed referral builds in the case where the referral code is embedded in the binary’s filename. In the case of iOS, `/promo/initialize/noua` is replaced by `/promo/initialize/ua`.

![refcode flow diagram](https://i.imgur.com/KQx2P67.png)

The record layouts are described below in Appendix A.

## How is this data processed server-side?

The server uses these pings to track retention for each referral group. In the case of referral codes which are associated with a publisher who is to be paid out for referrals, the server uses the referral pings to calculate payout and fraud signals. 

In the case of referral codes associated with a Brave partner, Brave may forward those publishers an aggregated report of referral events which are associated with their referral code. In the case of the creator referral program, the creator finds out how many users have installed Brave and used it 30 days later via their referral.

For referral codes associated with a publisher/creator who is getting payouts for referring users, information derived from the IP of the referred users may be retained for longer than 10 days for anti-fraud purposes. 

## Privacy considerations

Some referral codes require more tracking than others. For instance, for referral codes associated with publishers who are getting paid for referrals, we may want to persist IPs for over a month to do fraud analysis. We don't need to do this kind of tracking for "organic" referrals or other referrals that have no fraud incentive. Therefore, we should be careful to do the minimum tracking needed server-side for each referral category.

Another concern is that referral code in combination with other ping data could be a unique identifier. We should mitigate this risk by omitting “small” referral codes or grouping them into a single referrer category for pings as needed. For instance, it may be useful someday to send the week of install along with referral code. If only a single user downloaded Brave with a given referral code in a given week, this ping essentially would contain a unique identifier. In this case we should only include referral codes where it is very likely that many people on the same platform in the same geographic location installed Brave with that referral code in any given week.

Finally, we should make sure that third parties (creators and partners) aren’t able to abuse the referral mechanism in order to track individuals. If a creator created N publisher channels such as website domains or YouTube accounts, they could get a different referral code for each channel. They could then send a different referral code to N people and then login to their Brave publishers dashboards to check whether each of those people had downloaded Brave and were using it 30 days later. However this reveals relatively little information to them and is difficult to do at scale since it involves creating one channel and email address per user.

## Appendix A: Record Layout Examples

### Retrieval (Record layout A)

```
id            | 12327503
ts            | 2019-03-04 13:50:17.88564
platform      | winx64
referral_code | VNI569
iptags        | {}
```

#### Field Descriptions

Fields | Purpose | Lifecycle
------ | ------- | ---------
id | Auto-incrementing record identifier | Creation
ts | Auto-filled timestamp | Creation
platform | Platform identifier | Creation
referral_code | Client provided referral code | Creation
iptags | JSON structure containing ip derived data | Creation

### Initialization and Confirmation (Record layout B)

```
id                      | 4893845
ts                      | 2019-02-02 07:56:48.815515-05
referral_code           | EBO974
id_hash                 |
platform                | winx64
download_id             | 477d7e0b-7225-4dd9-ae54-df75dd944e94
download_ts             | 2019-02-02 07:56:48.815515-05
download_ping_count     | 1
download_last_ping_ts   | 2019-03-04 08:11:22.398203-05
finalized               | t
finalized_ts            | 2019-03-04 08:11:22.404668-05
eyeshade_confirmed      | t
eyeshade_confirmed_ts   | 2019-03-04 08:18:26.713592-05
iptags                  | {}
eyeshade_confirmed_txid | 891c1582-7a36-4327-ad21-8e35070b9849
```

#### Field Descriptions

Fields | Purpose | Lifecycle
------ | ------- | ---------
id | Auto-incrementing record identifier | Creation
ts | Auto-filled timestamp | Creation
referral_code | Client provided referral code | Creation
id_hash | Unused
platform | Platform identifier | Creation
download_id | Unique installation identifier | Creation
download_ts | Timestamp of record creation (same as `ts` ) | Confirmation
download_ping_count | Incrementing (should always be 1) - Note this was introduced with the intent to require more than a single confirmation call | Confirmation
download_last_ping_ts | Timestamp of last confirmation request | Confirmation
finalized | Boolean (default False) indicating confirmation status | Creation / Confirmation
finalized_ts | Timestamp of successful confirmation | Confirmation
eyeshade_confirmed | Boolean (default False) indicating eyeshade transfer status | Creation / Eyeshade transfer
eyeshade_confirmed_ts | Timestamp of successful Eyeshade transfer | Eyeshade transfer
iptags | JSON structure containing ip derived data | Creation
