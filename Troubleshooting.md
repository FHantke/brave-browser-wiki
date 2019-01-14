Build and browser start might fail for different reasons, some being:

## Database error

Similar to:

```
[48058:82435:0827/195241.326071:ERROR:database.cc(1821)] Web sqlite error 5, errno 0: database is locked, sql: SELECT COUNT(*) FROM sqlite_master
[48058:82435:0827/195241.326160:FATAL:statement.cc(50)] Cannot call mutating statements on an invalid statement.
```

If you experience something like the above, consider checking your processes and check if another browser instance is using the same profile. You can run `ps aux |grep Brave-Browser-Development` and check the console output. If something appears to you, try running `pkill -f Brave-Browser-Development` to kill these processes or consider restarting your machine and try again.

## `badTimezone` errors during `npm run init`

If you get errors like these while initializing the repo:

> error: object 5e6ecdad9f69b1ff789a17733b8edc6fd7091bd8: badTimezone: invalid author/committer line - bad time zone

make sure you don't have any of the following in your `~/.gitconfig`:

    [transfer]
        fsckobjects = true
    [fetch]
        fsckobjects = true
    [receive]
        fsckobjects = true

## Proxy warnings from depot_tools

To silence notices like these:

> NOTICE: You have PROXY values set in your environment, but gsutil in depot_tools does not (yet) obey them.
> Also, --no_auth prevents the normal BOTO_CONFIG environment variable from being used.
> To use a proxy in this situation, please supply those settings in a .boto file pointed to by the NO_AUTH_BOTO_CONFIG environment var.

Add this to `~/.bashrc`:

    export NO_AUTH_BOTO_CONFIG="${HOME}/.boto"

and put the following in `~/.boto`:

    [Boto]

## `npm run sync -- --all` fails

Did you remember to do a `git pull` of the main `brave-browser` repository first?

If it's still not working, try deleting your `src/out` directory.