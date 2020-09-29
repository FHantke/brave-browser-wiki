When you're just doing poor man debugging, you can do:

`LOG(ERROR) << "Simple logging";`

If you want to commit logs that will be useful over time:

`VLOG(1) << "[IPFS] Test";`


Then when you start you can match via file paths like this:

```
npm start -- --vmodule=*/ipfs/*=1 --enable-logging=stderr
```