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
Cache location             Local disk: "/Users/clifton/sccache"
Cache size                      19 GiB
Max cache size                 100 GiB
```

## Install pre-requisites
### Install rust
If you already have rust installed but you're not sure if it's current, you can run `rustup update`

#### Mac/Linux installation
Install Rust via [rustup](https://rustup.rs/)
```
curl https://sh.rustup.rs -sSf | sh
```
On Linux, you may need to install `libssl-dev` and `pkg-config` using apt/yum/dnf

#### Windows installation
Download and install Rust via [rustup](https://rustup.rs/) and follow the on=screen instructions


### Install sccache
With rust setup, you can install the `sccache` cargo package
```
cargo install --force sccache
```

### Troubleshooting
- You'll need to have OpenSSL installed on your machine
  - On macOS, you can install via Homebrew (`brew install openssl`), but some manual linking may be needed (for the include files)
  - Also on macOS, you can also run `brew doctor` to help try to identify problems with your installed packages
- If you are doing this on a new Mac, make sure you have XCode installed and that you've installed the command-line tools via `xcode-select --install`
- Clifton was getting an error `error: failed to run custom build command for 'rust-crypto v0.2.36'`, followed by errors about nested includes going too deep. The solution was to delete/move `/usr/local/include/stdint.h` (which must have been left over from an old install) which was referenced in the error
- When installing sccache v0.2.7 on Windows, you are likely to get an error `failed to compile`. This is a known error (https://github.com/mozilla/sccache/issues/292) and is due to a compilation warning issued during the build of ring-0.12.1 and the setting to treat warnings as errors. To get around this, in your cmd shell, `set _CL_=/wd5045` and `set _LINK_=/WX:NO` (this one may not be necessary), then rerun the cargo install command.

## Configuring sccache
You'll need to export some variables used by sccache to your `.bashrc`
```
export PATH="$PATH:~/.cargo/bin/"  # path to binaries like sccache
export SCCACHE_CACHE_SIZE=100G     # some of us use 100GB; you can use less if needed
export SCCACHE_DIR=~/sccache       # where the cache is physically stored
```

You may have to use `${HOME}` instead of `~` in the `PATH` variable in `.bashrc` since [it might break depot_tools](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md#install).

For convenience, you may also want to symlink the sccache binary into `/usr/local/bin`:
```
ln -s ~/.cargo/bin/sccache /usr/local/bin/sccache
```

With macOS, it's also handy to setup a launch agent that will re-launch `sccache` if it quits/dies. You can create a new file at `~/Library/LaunchAgents/com.sccache.agent.plist`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>EnvironmentVariables</key>
        <dict>
           <key>SCCACHE_DIR</key>
           <string>/Users/clifton/sccache</string>
           <!-- <key>SCCACHE_REDIS</key> -->
           <!-- <string>SCCACHE_REDIS_URL</string> -->
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
Be sure to update values there (such as cache size or the actual path to the binary) to match your install. Once that file is created, you can run the following:
```
launchctl load ~/Library/LaunchAgents/com.sccache.agent.plist
launchctl start ~/Library/LaunchAgents/com.sccache.agent.plist
```

And then finally, you can verify it's loaded by running:
```
launchctl list
```

## Setting the .npmrc
This is the most important part. In your `brave-browser` root directory, you'll want to open the `.npmrc` file. To enable `sccache`, you'll need to add a line:
```
sccache = sccache
```

For legacy purposes, it's important to note that this works with Muon also (and should work with any project really)