| Feature | Platforms | Rules | Navigation (same-site) | Navigation (cross-site) | Sub-resources (same-site) | Sub-resources (cross-site) |
| --- | --- | --- | --- | --- | --- | --- |
| URL clean copy | Desktop, Android | [clean-urls.json](https://github.com/brave/adblock-lists/blob/master/brave-lists/clean-urls.json) | NA | NA | NA | NA |
| [Query param stripping](https://brave.com/privacy-updates/5-grab-bag/#1-removing-known-tracking-parameters-from-urls) | All | [query_filter/utils.cc](https://github.com/brave/brave-core/blob/4b85ad298e22b5e0b5711aaf7cac3903db847439/components/query_filter/utils.cc#L24) | No | Yes | No | No |
| [Debouncing](https://brave.com/privacy-updates/11-debouncing/) | All | [debounce.json](https://github.com/brave/adblock-lists/blob/master/brave-lists/debounce.json) | No | Yes | No | No |
| [`removeparam`](https://github.com/gorhill/uBlock/wiki/Static-filter-syntax#removeparam) | All | various | Yes | Yes | Yes | Yes |