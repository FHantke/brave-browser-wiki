For details about adding a search engine, please see [Criteria for adding new search providers to Brave
](https://github.com/brave/brave-browser/wiki/Criteria-for-adding-new-search-providers-to-Brave)

## List of integrations
| Provider name | Added with | Referral code (part of query string) | Regions (2 digit country code) | Deployed in |
| ------------- | ---------- | ----------------------- | ------------------------------ | ----------- |
| **DuckDuckGo** | added long ago | `t=brave` | All | added long ago |
| **Ecosia** | Desktop/Android: https://github.com/brave/brave-core/pull/7494<br><br>iOS: TBD | Desktop: TBD<br><br>Android: `tt=42b8ae98`<br><br>iOS: `tt=d188c5da` | US, UK, GB, FR, DE, NL, BE, CH, SE | n/a |
| **Qwant** | Desktop/Android: https://github.com/brave/brave-core/pull/516<br><br>iOS: https://github.com/brave/brave-ios/pull/329<br>https://github.com/brave/browser-ios/pull/1716| `client=brz-brave` | All | Desktop: 0.55<br><br>Android: old tabs repo<br><br>iOS: 1.6.5 |
| **Startpage** | Desktop/Android: https://github.com/brave/brave-core/pull/6916 <br><br>Android: https://github.com/brave/brave-core/pull/7039<br><br> iOS: https://github.com/brave/brave-ios/pull/3003 | `segment=startpage.brave`| All | Desktop/Android: 1.16<br><br>iOS: 1.22 |
| **Yandex** | Desktop: https://github.com/brave/brave-core/pull/7090<br><br>Android: https://github.com/brave/brave-core/pull/7258<br> https://github.com/brave/brave-core/pull/7376<br><br>iOS: https://github.com/brave/brave-ios/pull/3050 | Android: `clid=2423859`<br><br>Desktop/iOS: `clid=2353835` | AM, AZ, BY, KG, KZ, MD, RU, TJ, TM, UZ | Desktop/Android: 1.18<br><br>iOS: 1.22 |

## Deprecated integrations
| Provider name | Added with | Referral code (part of query string) | Regions (2 digit country code) | Deployed in | Removed in |
| ------------- | ---------- | ----------------------- | ------------------------------ | ----------- | ------ |
| **Yahoo** | Desktop/Android https://github.com/brave/brave-core/pull/6201<br><br>iOS https://github.com/brave/brave-ios/pull/2755 | `fr=brave_yset` | AR, AT, AU, BR, CA, CH, CL, CO, DE, DK, ES, FI, FR, HK, ID, IE, IN, IT, MX, MY, NL, NO, NZ, PE, PH, SE, SG, TH, TW, GB, VE, VN | Desktop/Android: 1.12<br><br>iOS: 1.19.1 | TBD |