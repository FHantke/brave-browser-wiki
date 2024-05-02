# Copy clean link

The goal of the copy clean link feature is to remove anything that's not needed in the query string of a URL (typically attribution clutter like `utm_source=facebook`). This is achieved by removing the parameters present in a [manually-curated list](https://github.com/brave/adblock-lists/blob/master/brave-lists/clean-urls.json).

While the goal is different from the [privacy feature which removes query string trackers](https://github.com/brave/brave-browser/wiki/Query-String-Filter) automatically when Shields is enabled (link to wiki), a cleaned URL will also be [debounced](https://github.com/brave/brave-browser/wiki/Debouncing) and have its user trackers removed and so it will be at least as clean as what you would get as a result of visiting the URL in Brave.

## Implementation

[Implemented on desktop](https://github.com/brave/brave-browser/issues/23315) via right-click menu on the URL bar, Share, right-click on link. It is also [enabled by default for `Ctrl+C`](https://github.com/brave/brave-browser/issues/26761) [except on macOS](https://github.com/brave/brave-browser/issues/29303) due to focus problems on that platform. Support for the keyboard shortcut can be toggled via `brave://flags/#brave-copy-clean-link-by-default`.

On macOS, it's also in the [application menu](https://github.com/brave/brave-browser/issues/26825).

On iOS, it is available via [a long-press on links](https://github.com/brave/brave-ios/issues/6179) and the [share option](https://github.com/brave/brave-ios/issues/8070). The same is true on [Android](https://github.com/brave/brave-browser/issues/26013).

### Issue trackers

Engineering issues: https://github.com/brave/brave-browser/issues?q=is%3Aissue+label%3Acopy-clean-link+

List issues: https://github.com/brave/adblock-lists/issues?q=is%3Aissue+label%3A%22Clean+URLS%22

## Writing new filters

Anybody can suggest new additions to the list by [submitting a PR](https://github.com/brave/adblock-lists/compare).

### Basic requirements

- Include an example URL in the PR and/or commit message.
- The original and cleaned URLs must lead to the same page.
- Cannot break the page.
- Cannot change the page functionality (e.g. causing the page to be potentially shown in a different language by removing a language parameter).
- Only HTTP/HTTPS URLs can be targeted.

### Tips

- Avoid the global scope (`*://*/*`) for parameters that are too generic (i.e. likely to be used for different purposes on different sites).
- Avoid changing the last element of JSON arrays to make `git blame` easier. Add to the beginning instead.
- Keep the test entries last in the file.
- Look for the parameters in the [Adguard list](https://github.com/AdguardTeam/AdguardFilters/tree/master/TrackParamFilter/sections) to see whether any of them are commented-out in that list due to breakage.