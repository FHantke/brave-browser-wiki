[More information about sccache](https://github.com/mozilla/sccache)

## Install pre-requisites
### Install rust
If you already have rust installed but you're not sure if it's current, you can run `rustup update`
#### Mac/Linux installation
Install Rust via [rustup](https://rustup.rs/)
```
curl https://sh.rustup.rs -sSf | sh
```
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

## Configuring sccache
You will want to add 
```
# On Linux this is [~/.cargo/bin]
export PATH=$PATH:<the directory containing sccache>
```
to your `.bashrc` and
```
sccache = <path to sccache>
```
to your `.npmrc`.

**NOTE**: On Linux this will be something like `sccache = ~/.cargo/bin/sccache`, I used a full path but you might get away with `sccache = sccache` as well as long as your cargo bin folder is in your PATH as described above.

## Setting the .npmrc
This is the most important part. In your `brave-browser` root directory, you'll want to open the `.npmrc` file. To enable `sccache`, you'll need to add a line:
```
sccache = sccache
```