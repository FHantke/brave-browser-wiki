[More information about sccache](https://github.com/mozilla/sccache)

## Setup (Mac/Linux)
Install Rust via [rustup](https://rustup.rs/)
```
curl https://sh.rustup.rs -sSf | sh
```
## Install sccache
```
cargo install --force sccache
```

## Setup sccache
You will want to add 
```
# on linux this is [~/.cargo/bin]
export PATH=$PATH:<the directory containing sccache>
```
to your .bashrc and
```
sccache = <path to sccache>
```
to your .npmrc.

**NOTE**: on linux this will be something like `sccache = ~/.cargo/bin/sccache`, I used a full path but you might get away with `sccache = sccache` as well as long as your cargo bin folder is in your PATH as described above.