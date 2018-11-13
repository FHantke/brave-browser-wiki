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
`[path-to-your-package] -- --rewards=[options]`

### Options
Option name | Option value | Description
------------ | ------------- | -------------
env | `stag` or `prod` | You can use this option for switching between production and staging servers
reconcile-interval | whole number | Define what the interval should be between monthly contributions in minutes.

### Usage
(replace ... with examples from How to run it)

* `... -- --rewards=env=stag` (single)
* `... -- --rewards=env=stag,reconcile-interval=5` (multiple)