Note: This document is a complement to the referral system described at [Brave's Use of Referral Codes](https://github.com/brave/brave-browser/wiki/Brave%E2%80%99s-Use-of-Referral-Codes)

# Why does Brave capture usage data?

Usage data is used to guide business decisions, inform product development and support resource allocation.

# What are usage requests?

A usage ping is a HTTP GET request, with data encoded in query parameters, used to count active users. The encoded request is sent to laptop-updates.brave.com either when the browser starts for the first time in a day, or after midnight if the browser is left open. It contains no personal data or personally identifying information.

The details of the query parameters in the request are defined below.

The request is routed to a CDN before it is forwarded to Brave for capture. The CDN performs the following:

* Infers country code from IP address (countries with a small number of users < 1000 are excluded from this inference step).
* Infers region code (https://en.wikipedia.org/wiki/ISO_3166-2) from IP address.
* Removes IP address from data sent to Brave.

The CDN-modified request is sent to Brave for data capture. Brave performs the following:

* The current date (year_month_day) is added to the request and the usage request is stored.

Once a day Brave aggregates the captured data, storing it for analysis and reporting.

# What information is contained in a usage request?

The browser provides the following values in the query parameters of the request:

| Field | Description | Values | Example |
| ----- | ----------- | ------ | ------- |
| platform | Platform identifier | winx64-bc, winia32-bc, winarm64-bc, linux-bc, osx-bc, osxarm64-bc, android-bc, ios | winx64-bc |
| channel | Channel identifier | Nightly, dev, developer, beta, release | release |
| version | Version number | x.x.x semver formatted string | 1.2.3 |
| daily | Daily usage flag; reported as true if used today | true or false | true |
| weekly | Weekly usage flag; reported as true if used for the first time this week | true or false | true |
| monthly | Monthly usage flag; reported as true if used for the first time this month | true or false | false |
| first | First day of installation flag | true or false | false |
| woi | Week of installation; date of the first Monday before the installation date | YYYY-MM-DD formatted date | 2020-11-16 |
| ref | Referral code (now limited to a small set of 50 to 100 affiliated referrers if this Brave instance was downloaded via a referral link) | ABC123 formatted string or the value none | ABC123 |
| dtoi | Date of browser installation. Only reported in the 30 days after installation. | YYYY-MM-DD formatted date | 2020-11-18 |
| adsEnabled | Ads status flag | true or false | true |
| wallet2 | 3-bit bit field for monthly, weekly, daily usage. The most significant bit indicates Wallet usage in the current month, and the least significant bit indicates daily usage in the current day. | 0 to 7 | 7 for daily, weekly and monthly usage, 4 for monthly usage, 2 for weekly usage, 1 for daily usage
| arch | CPU architecture Brave is compiled for | X86, x86_64, ia64, arm64, ppc64 | x86_64 |

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


