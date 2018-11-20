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

# Disabling tests

You can disable a flacky test by prefixing it with `DISABLED_`.  Whenever doing this though, make sure to post an issue to re-enable it and assign it to the owner of the test. 

```
- TEST(ExampleTest, CrashingTest) {
+ TEST(ExampleTest, DISABLED_CrashingTest) {
```

# Other targets

Other targets can be passed to `npm run test -- <suite>` such as running Chrome's tests.

Example:

`npm run test -- browser_tests` will run a lot of tests, but some will not pass.

# Useful Resources

## Chromium test frameworks

- [GoogleTest Primer](https://github.com/google/googletest/blob/master/googletest/docs/primer.md)
- [GoogleMock for Dummies](https://github.com/google/googletest/blob/master/googlemock/docs/ForDummies.md)

# Manual tests

## Privacy network audit

Run:

`npm run test-security`

Or if testing a non-Debug build:

`npm run test-security -- --output_path="/path/to/brave/binary"`

This will start the browser for 2 minutes. During that time, try doing some actions in Brave other than going to a webpage (since this will create a lot of false positives). For instance:
1. open preferences and change some
2. go through the brave://welcome onboarding process
3. enable brave rewards and claim a grant


Or manually:

1. Download Brave or `npm start`
2. Open it with these command line flags `--log-net-log=/path/to/somefile.json --net-log-capture-mode=IncludeSocketBytes`. for instance on my mac it's `open /Applications/Brave\ Browser.app --args --log-net-log=/Users/yan/chromelog4.json --net-log-capture-mode=IncludeSocketBytes`.  If you're using `npm start` add the arguments to `lib/start.js`.
3. Try doing some actions in Brave other than going to a webpage.
4. Close brave, open brave, go to chrome://net-internals and pick the option to import the JSON file from step 
5. Inspect requests that say `URL_REQUEST` and you can actually see what they are sent to.

Note requests that return 307 are not actually sent over the network

---

`GOOGLE_OWNED_SERVICE` is a very good search term to find all the places in Chromium that hit Google servers for any reason. https://cs.chromium.org/search/?q=%22destination:+GOOGLE_OWNED_SERVICE%22&sq=package:chromium&type=cs


# Debugging tips

It's sometimes useful to see the console while browser tests are running.  When it is in this state, you can't enter user input or focus the window, but you can right click and `Inspect` to see the console and show the devtools.