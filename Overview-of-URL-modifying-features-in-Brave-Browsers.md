| Feature | Platforms | Rules | Navigation (same-site) | Navigation (cross-site) | Sub-resources (same-site) | Sub-resources (cross-site) |
| --- | --- | --- | --- | --- | --- | --- |
| URL clean copy | Desktop, Android | [clean-urls.json](https://github.com/brave/adblock-lists/blob/master/brave-lists/clean-urls.json) | NA | NA | NA | NA |
| [Query param stripping](https://github.com/brave/brave-browser/wiki/Query-String-Filter) | All | [query_filter/utils.cc](https://github.com/brave/brave-core/blob/4b85ad298e22b5e0b5711aaf7cac3903db847439/components/query_filter/utils.cc#L24) | No | Yes | No | Yes |
| [Debouncing](https://github.com/brave/brave-browser/wiki/Debouncing) | All | [debounce.json](https://github.com/brave/adblock-lists/blob/master/brave-lists/debounce.json) | No | Yes | No | No |
| [`removeparam`](https://github.com/gorhill/uBlock/wiki/Static-filter-syntax#removeparam) | All | various | Yes | Yes | Yes | Yes |
| HTTPS Upgrades | Desktop, Android | [https-upgrade-exceptions-list.txt](https://github.com/brave/adblock-lists/blob/master/brave-lists/https-upgrade-exceptions-list.txt) | Yes | Yes | Yes | Yes |