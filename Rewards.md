# Contribution
# Flags
This are flags that you can use for rewards. Flags can help you out testing things out, so that you don't need to wait that long or switch between things.

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