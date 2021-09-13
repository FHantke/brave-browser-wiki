Note: This document is a complement to the referral system described at https://github.com/brave/brave-browser/wiki/Brave%E2%80%99s-Use-of-Referral-Codes

# Why does Brave capture usage data?

Usage data is used to guide business decisions, inform product development and support resource allocation.

# What are usage requests?

A usage ping is a HTTP GET request, with data encoded in query parameters, used to count active users. The encoded request is sent to laptop-updates.brave.com either when the browser starts for the first time in a day, or after midnight if the browser is left open. It contains no personal data or personally identifying information.

Note: All fields are described in the table below with example values.

The request is encoded with the following rules:

* The platform, channel, and version strings are retrieved from browser state.
* The daily flag is set to true.
* The weekly flag is set to true if the browser is used for the first time in a calendar week, starting on Monday.
* The monthly flag is set to true if the browser is used for the first time in a calendar month, starting on the 1st of the month.
* The first flag is set to true if this is the first day the browser was installed.
* The woi (week of installation), ref (referral code, not used if default), and dtoi (date of installation) are retrieved from browser state.
* The adsEnabled flag, set to true if ads are enabled in the browser.
* The walletActive flag. Set to 7 for daily, 6 for weekly, 4 for monthly, 0 if wallet never used.
* The arch field, which has the CPU architecture Brave was compiled for. This is sent only if ads are enabled in the browser.

The request is routed to a CDN before it is forwarded to Brave for capture. The CDN performs the following:

* Infers country code from IP address (countries with a small number of users < 1000 are excluded from this inference step).
* Infers region code (https://en.wikipedia.org/wiki/ISO_3166-2) from IP address.
* Removes IP address from data sent to Brave.

The CDN-modified request is sent to Brave for data capture. Brave performs the following:

* The current date (year_month_day) is added to the request and the usage request is stored.

Once a day Brave aggregates the captured data, storing it for analysis and reporting.

# What information is contained in a usage request?

| Field | Description | Values | Source | Example |
| ----- | ----------- | ------ | ------ | ------- |
| year_month_day | Date of request capture | YYYY-MM-DD encoded date | Brave | 2020-11-19 |
| platform | Platform identifier | Winx64-bc, winia32-bc, linux-bc, osx-bc, android-bc, ios | Browser| winx64-bc |
| channel | Channel identifier | Nightly, dev, developer, beta, release | Browser | release |
| version | Version number | x.x.x semver formatted string | Browser | 1.2.3 |
| daily | Daily flag | true or false | Browser | true |
| weekly | Weekly flag | true or false | Browser | true |
| monthly | Monthly flag | true or false | Browser | false |
| first | First day of installation flag | true or false | Browser | false |
| woi | Week of installation - date of the first Monday before the installation date | YYYY-MM-DD formatted date | Browser | 2020-11-16 |
| ref | Referral code (now limited to a small set of 50 to 100 affiliated referrers if this Brave instance was downloaded via a referral link) | ABC123 formatted string or the value none | Browser | ABC123 |
| country_code | Country Code | 2 digit string containing the country code | CDN | US |
| region | State / Province Code | 2 digit string containing a sub-national region code | CDN | CA |
| dtoi | Date of installation - date of the browser installation (held in browser state for 14 days then removed) | YYYY-MM-DD formatted date | Browser | 2020-11-18 |
| adsEnabled | Ads status flag | true or false | Browser | true |
| walletActive | Brave Wallet Status | 4 possible values for daily user, weekly user, monthly user, never used. | Browser | 7
| arch | CPU architecture Brave is compiled for | X86, x86_64, ia64, arm64, ppc64 | Browser | x86_64 |

# What aggregated data is built from the usage requests?

| Metric | Description | Rule |
| ------ | ----------- | ---- |
| DAU | Daily active users | Count of requests for a year_month_day |
| DNU | Daily new users| Count of requests for a year_month_day where daily is true |
| DRU | Daily returning users | Count of requests for a year_month_day where daily is false |
| WAU | Weekly active users | Count of requests for a year_month_day where weekly is true |
| WNU | Weekly new users | Count of requests for a year_month_day where weekly is true and first is true |
| WRU | Weekly returning users | Count of requests for a year_month_day where weekly is false |
| MAU | Monthly active users | Count of requests for a year_month_day where monthly is true |
| MNU | Monthly new users | Count of requests for a year_month_day where monthly is true and first is true |
| MRU | Monthly returning users | Count of requests for a year_month_day where monthly is true and first is false |
| Daily retention | Percentage of installations that use the browser a specific number of days after installation (capped at 14 days) | Count of requests for a year_month_day where dtoi is X, divided by the number of requests on X in which first is true |
| Weekly retention | Percentage of installations that use the browser at least one time in a calendar week compared to the number installed on a target week | Count of requests for year_month_day where woi is X, divided by the number of requests in the week starting on X in which first is true |

# Privacy Considerations

The Brave usage counting system is specifically designed to not rely on personally identifying information. The system builds usage counts by aggregating the usage requests sent over the course of a single calendar day.


