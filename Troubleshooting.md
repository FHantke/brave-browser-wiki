Build and browser start might fail for different reasons, some being:

## Database error

Similar to:

```
[48058:82435:0827/195241.326071:ERROR:database.cc(1821)] Web sqlite error 5, errno 0: database is locked, sql: SELECT COUNT(*) FROM sqlite_master
[48058:82435:0827/195241.326160:FATAL:statement.cc(50)] Cannot call mutating statements on an invalid statement.
```

If you experience something like the above, consider checking your processes and check if another browser instance is using the same profile. You can run `ps aux |grep Brave-Browser-Development` and check the console output. If something appears to you, try running `pkill -f Brave-Browser-Development` to kill these processes or consider restarting your machine and try again.