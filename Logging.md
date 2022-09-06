When you're just doing poor man debugging, you can do:

`LOG(ERROR) << "Simple logging";`

If you want to commit logs that will be useful over time:

`VLOG(1) << "[IPFS] Test";`


Then when you start you can match via file paths like this:

```
npm start -- --vmodule=*/ipfs/*=1 --enable-logging=stderr
```

`VLOG` should be used for debug info not `LOG(ERROR)`.

Sensitive information should never be logged at all. For example, private keys, OAuth tokens, HTTP basic auth credentials, cookies.