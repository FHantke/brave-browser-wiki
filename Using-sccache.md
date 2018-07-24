[More information about sccache](https://github.com/mozilla/sccache)

## Setup
### Mac/Linux
Install Rust via [rustup](https://rustup.rs/)
```
curl https://sh.rustup.rs -sSf | sh
```
### Windows
Download and install Rust via [rustup](https://rustup.rs/) and follow the onscreen instructions
## Install sccache
```
cargo install --force sccache
```

## Setup sccache
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