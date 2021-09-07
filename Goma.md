In order to access the Brave Goma server, you must be given access (see the list on https://goma.brave.com) and then set it up like this.

In `~/.npmrc`:
```
goma_server_host=goma.brave.com
```

In `~/.bashrc`:

```
export GOMA_FALLBACK=1
export GOMA_USE_LOCAL=1
export GOMA_LOCAL_OUTPUT_CACHE_DIR="${HOME}/.goma_cache"
export GOMA_LOCAL_OUTPUT_CACHE_MAX_CACHE_AMOUNT_IN_MB="102400"
export GOMA_LOCAL_OUTPUT_CACHE_THRESHOLD_CACHE_AMOUNT_IN_MB="81920"
```