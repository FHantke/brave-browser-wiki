# Overview

There are 2 types of tests currently in use by Brave: unit tests and browser tests.

# Unit tests

Unit tests are compiled C++ tests that test a specific function or thing.

```
npm run test -- brave_unit_tests
```

Typical output looks like this:


```
$ npm run test -- brave_unit_tests

> brave@1.0.0 test /Users/bbondy/projects/brave/brave
> node ./scripts/commands.js test "brave_unit_tests"

ninja -C /Users/bbondy/projects/brave/brave/src/out/Release brave_unit_tests
ninja: Entering directory `/Users/bbondy/projects/brave/brave/src/out/Release'
[59/59] LINK ./brave_unit_tests
/Users/bbondy/projects/brave/brave/src/out/Release/brave_unit_tests --enable-logging --v=0
IMPORTANT DEBUGGING NOTE: batches of tests are run inside their
own process. For debugging a test inside a debugger, use the
--gtest_filter=<your_test_name> flag along with
--single-process-tests.
Using sharding settings from environment. This is shard 0/1
Using 8 parallel jobs.
[1/9] BraveSiteHacksNetworkDelegateHelperTest.NoChangeURL (1 ms)
[2/9] BraveSiteHacksNetworkDelegateHelperTest.RedirectsToEmptyDataURLs (1 ms)
[3/9] BraveSiteHacksNetworkDelegateHelperTest.RedirectsToStubs (1 ms)
[4/9] BraveSiteHacksNetworkDelegateHelperTest.Blocking (1 ms)
[5/9] BraveStaticRedirectNetworkDelegateHelperTest.NoModifyTypicalURL (2 ms)
[6/9] BraveStaticRedirectNetworkDelegateHelperTest.ModifyGeoURL (1 ms)
[7/9] ChromeImporterTest.ImportHistory (4 ms)
[8/9] ChromeImporterTest.ImportBookmarks (4 ms)
[9/9] ChromeImporterTest.ImportFavicons (2 ms)
SUCCESS: all tests passed.
Tests took 0 seconds.
```

# Browser tests

Browser tests compile a browser executable together with the test C++ code.  This type of test is useful for when you need to test something that requires most of the browser services started.

```
npm run test -- brave_browser_tests
```

Typical output looks like this:

```
$ npm run test -- brave_browser_tests

> brave@1.0.0 test /Users/bbondy/projects/brave/brave
> node ./scripts/commands.js test "brave_browser_tests"

ninja -C /Users/bbondy/projects/brave/brave/src/out/Release brave_browser_tests
ninja: Entering directory `/Users/bbondy/projects/brave/brave/src/out/Release'
[1/1] Regenerating ninja files
[59/59] LINK ./brave_browser_tests
/Users/bbondy/projects/brave/brave/src/out/Release/brave_browser_tests --enable-logging --v=0
IMPORTANT DEBUGGING NOTE: each test is run inside its own process.
For debugging a test inside a debugger, use the
--gtest_filter=<your_test_name> flag along with either
--single_process (to run the test in one launcher/browser process) or
--single-process (to do the above, and also run Chrome in single-process mode).
Using sharding settings from environment. This is shard 0/1
Using 4 parallel jobs.
[1/4] AdBlockServiceTest.NotAdsDoNotGetBlocked (2107 ms)
[2/4] AdBlockServiceTest.AdsGetBlocked (2370 ms)
[3/4] HTTPSEverywhereServiceTest.NoRedirectsNotKnownSite (2369 ms)
[4/4] HTTPSEverywhereServiceTest.RedirectsKnownSite (2370 ms)
SUCCESS: all tests passed.
```
# Filtering test targets

`npm run test -- brave_browser_tests --filter=BraveContentSettingsObserverBrowserTest.*`


# Running tests in release mode

`npm run test -- brave_browser_tests Release`

# Other targets

Other targets can be passed to `npm run test -- <suite>` such as running Chrome's tests.

Example:

`npm run test -- browser_tests` will run a lot of tests, but some will not pass.

# Useful Resources

## Chromium test frameworks

- [GoogleTest Primer](https://github.com/google/googletest/blob/master/googletest/docs/Primer.md)
- [GoogleMock for Dummies](https://github.com/google/googletest/blob/master/googlemock/docs/ForDummies.md)