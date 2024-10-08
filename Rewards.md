# (OUTDATED)
# Contribution

## How does it work?
Contribution is divided in two phases. Phase 1 handles BAT transaction, where phase 2 handles sending votes for publishers that are part of your contribution to the server.

### Phase 1
We will first determinate if you have enough funds and if you have any publishers on your list. If all is good on that front we will start contribution process. We will do back and forward calls between client and the server to ensure that we anonize your data, before we send any publishers to the server. At the last step we will also transfer your selected amount for the contribution from your wallet.

### Phase 2
Phase 2 is all about publisher that are part of your contribution. In phase 1 we got surveyors which we will now use for voting process. First we will determinate which publishers should get any BAT. After that we will assign all surveyors (votes) to the winning publishers, batch it together (so that we are not calling server for every vote) and then we send batches to the server.


## Retries
We always try to process contribution as soon as possible and as reliable as possible. But of course sometimes things don't go as we want. Because of that we have re-try logic in place that handles this scenarios. Every phase has a different logic.

#### Phase 1
We will re-try contribution in following intervals. If all intervals will fail we will terminate contribution and show a notification to the user that contribution failed.
* 1 hour
* 6 hours
* 12 hours
* 24 hours
* 48 hours

#### Phase 2
We will re-try contribution in following intervals. Contrary to the phase 1 this phase will not terminate contribution if you go through all intervals. When you are at the last interval (24h) we will just repeat this interval until we are successful.
* 1 hour
* 6 hours
* 24 hours


# Flags
This are flags that you can use for rewards. Flags can help you out testing things, so that you don't need to wait that long or switch between things.

### How to run it

#### Running it from source
`npm run start -- --rewards=[options]`

#### Packaged version
`[path-to-your-package] --rewards=[options]`

### Options
Option name | Option value | Description
------------ | ------------- | -------------
staging | boolean | You can use this option for switching between production and staging servers
reconcile-interval |integer | Define what the interval should be between monthly contributions in minutes.
short-retries | boolean | Enables short retries intervals that can be used for testing contribution failures 
current-country | string | Indicates the country that this installation should be considered. Currently, only "JP" is effective but any string value will be evaluated.

### Usage
(replace ... with examples from How to run it)

* `... -- --rewards=staging=false` (single)
* `... -- --rewards=staging=false,reconcile-interval=5` (multiple)

# Logging
We have two 6 levels of logging. Difference between them is in how detailed and severe log is. Bellow you can see the table.

```
  LOG_ERROR = 1,
  LOG_WARNING = 2,
  LOG_INFO = 3,
  LOG_DEBUG = 4,
  LOG_REQUEST = 5,
  LOG_RESPONSE = 6
```

You can trigger level 1-3 with adding flag `--v=num`. `Num` should be replaced with number 1, 2 or 3. If you want to go even deeper you can use flag `--vmodule=*rewards*=num`, where you replace `num` with 4, 5 or 6. This two flags can be used at the same time like `--v=3 ----vmodule=*rewards*=6` if you want to print out all logs (levels 1-6).