[More information about sccache](https://github.com/mozilla/sccache)

`sccache` helps save a lot of time by caching the object files you compile. If something triggers a rebuild (ex: you delete the `src/out` directory), `sccache` will try to use its cache.  Once installed, you can see the number of hits/misses by running `sccache -s`.  You'll see an output like this:
```
Compile requests              9144
Compile requests executed     9082
Cache hits                    8051
Cache misses                  1031
Cache timeouts                   0
Cache read errors                0
Forced recaches                  0
Cache write errors               0
Compilation failures             0
Cache errors                     0
Non-cacheable compilations       0
Non-cacheable calls             62
Non-compilation calls            0
Unsupported compiler calls       0
Average cache write          0.009 s
Average cache read miss      8.833 s
Average cache read hit       0.025 s
Cache location             Local disk: "/Users/bsclifton/sccache"
Cache size                      19 GiB
Max cache size                 100 GiB
```

## Table of contents
- [Install pre-requisites](#install-pre-requisites)
- [Configuring sccache](#configuring-sccache)
  - [Shared S3 cache](#shared-s3-cache)
  - [Local disk cache](#local-disk-cache)
- [Configuring brave-browser to use sccache](#configuring-brave-browser-to-use-sccache)
- [(macOS) configuring sccache to restart](#macos-configuring-sccache-to-restart)

## Install pre-requisites
### Install rust
If you already have rust installed but you're not sure if it's current, you can run `rustup update`

#### Mac/Linux installation
Install Rust via [rustup](https://rustup.rs/)
```bash
curl https://sh.rustup.rs -sSf | sh
```
On Linux, you may need to install `libssl-dev` and `pkg-config` using apt/yum/dnf

#### Windows installation
Download and install Rust via [rustup](https://rustup.rs/) and follow the on-screen instructions


### Install sccache
With rust setup, you can install the `sccache` cargo package
```bash
cargo install --force sccache
```

### Paths
You'll want to make sure rust / cargo binaries can be found by modifying your path in your shell profile, e.g. ~/.bashrc:
```bash
export PATH="$PATH:$HOME/.cargo/bin"
```
You may have to use `${HOME}` instead of `~` in the `PATH` variable in `.bashrc` since [it might break depot_tools](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md#install).

For convenience, you may also want to symlink the sccache binary into `/usr/local/bin`:
```bash
ln -s ~/.cargo/bin/sccache /usr/local/bin/sccache
```

Test it's working by running `which sccache` and observing that your shell has found the program.

### Troubleshooting the install
- You'll need to have OpenSSL installed on your machine
  - On macOS, you can install via Homebrew (`brew install openssl`), but some manual linking may be needed (for the include files)
  - Also on macOS, you can also run `brew doctor` to help try to identify problems with your installed packages
- If you are doing this on a new Mac, make sure you have XCode installed and that you've installed the command-line tools via `xcode-select --install`
- Clifton was getting an error `error: failed to run custom build command for 'rust-crypto v0.2.36'`, followed by errors about nested includes going too deep. The solution was to delete/move `/usr/local/include/stdint.h` (which must have been left over from an old install) which was referenced in the error
- When installing sccache v0.2.7 on Windows, you are likely to get an error `failed to compile`. This is a known error (https://github.com/mozilla/sccache/issues/292) and is due to a compilation warning issued during the build of ring-0.12.1 and the setting to treat warnings as errors. To get around this, in your cmd shell, `set _CL_=/wd5045` and `set _LINK_=/WX:NO` (this one may not be necessary), then rerun the cargo install command.
- Using sccache v0.2.8, you may notice that nothing is cacheable anymore when looking at `sccache --show-stats`:

      Non-cacheable reasons:
      Can't handle UnknownFlag arguments with -Xclang
To fix this, reinstall v0.2.7: `cargo install -f --git https://github.com/petemill/sccache.git --rev 5ce26c30c16ecd14ff16ec53bb242340ca402423`

If you can't compile v0.2.7 because your operating system's version of OpenSSL is too new: `cargo install -f --git https://github.com/antonok-edm/sccache.git --rev 2bf7ff480fbd70e43c167af193a4fc68d385df3a`


## Configuring sccache

You'll need to export some variables used by sccache to your `.bashrc`. Sccache is aimed to be distributed so it supports a number of providers: s3, redis, etc, as well as a local disk cache for only caching your own output.

### Shared S3 cache
This is a great option if compiling takes a while on your machine. The cache is stored in an S3 bucket and multiple people can read/write to the bucket as they are compiling to take advantage of the cache.
```bash
# s3 cache
export SCCACHE_BUCKET=sccache-macos-bucket # bucket must exist and your access key must have read/write perm
```

If using S3, you will need to ask for credentials (and the bucket name) from the Release Engineering team (see #devops in Slack). Once you have this information, you can store your AWS credentials in `~/.aws/credentials` like so:
```
[default]
aws_access_key_id = XXXXXXXXXXXXX
aws_secret_access_key = XXXXXXXXXXXXXXXXXXXXXXXXXXX
```
You should `chmod 600 ~/.aws/credentials` and review our [AWS Access Key guidelines](https://github.com/brave/devops/wiki/Developing-With-AWS-Access-Keys) for more info. 

(For those who are curious: `aws-vault` will work as well, but it's not recommended for sccache credentials because it has some usability problems. You will need to make sure that the sccache server daemon is started with `aws vault`, e.g. `aws-value exec <profile> -- sccache --start-server` before starting any builds, otherwise it will be automatically started but won't have the right credentials, and all cache writes will fail. Also, note that the server will stop on its own after 10 minutes of inactivity. If you figure out a way to make this work that isn't so painful, please update this page!)

_**NOTE**_: if you use aws-cli then you may have multiple profiles. If you set the `default` profile to the `sccache` credentials, you should be good to go. Otherwise, `sccache` should support multiple profiles via environment variable. Try setting either `AWS_PROFILE` or `AWS_DEFAULT_PROFILE` to the name of the profile you'd like to use before you compile.

### Local disk cache
If your machine is pretty powerful you can still benefit from sccache. Local disk cache is a nice way to optimize your setup. It can take up quite a bit of space though (make sure you have enough room).
```bash
# disk cache
export SCCACHE_CACHE_SIZE=100G     # some of us use 100GB; you can use less if needed
export SCCACHE_DIR=~/sccache       # where the cache is physically stored
```

## Starting / stopping sccache
sccache is running as a daemon; before you start a compile you need to start it with:
`sccache --start-server`

If you need to stop it, you can stop by using the following:
`sccache --stop-server`

At any time you can pull up the stats for sccache using:
`sccache -s`

You'll see hits (object file will be downloaded from cache) and misses (computer will compile and upload to cache)

## Configuring brave-browser to use sccache

### Setting the environment variable
This is the most important part. In your `brave-browser` root directory, you'll want to open the `.npmrc` file. To enable `sccache`, you'll need to add a line:
```
sccache = /path/to/sccache
```
This will make sure when running any npm script in brave-browser that the `sccache` environment variable is set to the string `"sccache"`. This could instead be set in any other way: via an export in `~/.npmrc` or even via `.bashrc`.

## (macOS) configuring sccache to restart
If you're running macOS and sick of sccache dying and then failing to start automatically with the build, you can do the following:

1. Create a file `com.sccache.agent.plist`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>EnvironmentVariables</key>
        <dict>
           <key>SCCACHE_DIR</key>
           <string>/Users/bsclifton/sccache</string>
           <key>SCCACHE_CACHE_SIZE</key>
           <string>100G</string>
        </dict>
        <key>Label</key>
        <string>com.sccache.agent</string>
        <key>ProgramArguments</key>
        <array>
          <string>/usr/local/bin/sccache</string>
          <string>-s</string>
        </array>
        <key>StandardErrorPath</key>
        <string>/dev/null</string>
        <key>StandardOutPath</key>
        <string>/dev/null</string>
        <key>StartInterval</key>
        <integer>60</integer>
</dict>
</plist>
```

2. Edit the VALUE for `SCCACHE_DIR` (ex: replace `/Users/bsclifton/sccache` with a folder you'd like to use for your cache storage. Note that if you don't have a custom config on the storage path and just want to use the default cache path (default path is under `/Users/bsclifton/Library/Caches/sccache`), it is not necessary to have `SCCACHE_DIR` key and its string config here.
3. Edit the path (under `ProgramArguments`) for the `sccache` binary (if needed). For example, mine is shown in config as `/usr/local/bin/sccache` (which is a symlink I created using `ln -s /Users/bsclifton/.cargo/bin/sccache /usr/local/bin/sccache`)
4. Put this file into place at ~/Library/LaunchAgents/com.sccache.agent.plist
5. `launchctl load ~/Library/LaunchAgents/com.sccache.agent.plist`
6. `launchctl start ~/Library/LaunchAgents/com.sccache.agent.plist`
7. `launchctl list` to check your service is launched or not


This will basically just ping sccache once a minute to keep it alive (or restart it it died). 

Taken from https://bravesoftware.slack.com/archives/C7VLGSR55/p1521752407000424 and other notes (thanks [@bridiver](https://github.com/bridiver), [@darkdh](https://github.com/darkdh))