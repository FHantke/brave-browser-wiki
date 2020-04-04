Android and Linux
```
npm run init
Open Terminal
cd to build/rustup/0.1.2
export RUSTUP_HOME=<src_root>/build/rustup/0.1.2
export CARGO_HOME=<src_root>/build/rustup/0.1.2
./rustup update
cd ..
m -rf downloads
m -rf git
m -rf tmp
m -rf registry/src
cd ..
mv 0.1.2 0.1.3
tar zcvfp rust_deps_linux_0.1.3.gz *
Login to https://signin.aws.amazon.com/signin
Navigate to Services
Select `S3` located under Storage
Select `rust-pkg-brave-core`
Select `Upload`
Upload `rust_deps_linux_0.1.3.gz` to https://s3-us-west-2.amazonaws.com/rust-pkg-brave-core
Set the new version in `script/rust_deps_config.py` and create a pull request for your changes
```

iOS and macOS
```
npm run init
Open Terminal
cd to build/rustup/0.1.2
export RUSTUP_HOME=<src_root>/build/rustup/0.1.2
export CARGO_HOME=<src_root>/build/rustup/0.1.2
./rustup update
cd ..
m -rf downloads
m -rf git
m -rf tmp
m -rf registry/src
cd ..
mv 0.1.2 0.1.3
tar zcvfp rust_deps_mac_0.1.3.gz *
Login to https://signin.aws.amazon.com/signin
Navigate to Services
Select `S3` located under Storage
Select `rust-pkg-brave-core`
Select `Upload`
Upload `rust_deps_mac_0.1.3.gz` to https://s3-us-west-2.amazonaws.com/rust-pkg-brave-core
Set the new version in `script/rust_deps_config.py` and create a pull request for your changes
```

Windows
```
npm run init
Open "Command Prompt"
cd to build/rustup/0.1.2
set RUSTUP_HOME=<src_root>/build/rustup/0.1.2
set CARGO_HOME=<src_root>/build/rustup/0.1.2
./rustup update
cd ..
rmdir /S downloads
rmdir /S git
rmdir /S tmp
rmdir /S registry/src
cd ..
ren 0.1.2 0.1.3
cd 0.1.3
powershell Compress-Archive . rust_deps_win_0.1.3.zip
Login to https://signin.aws.amazon.com/signin
Navigate to Services
Select `S3` located under Storage
Select `rust-pkg-brave-core`
Select `Upload`
Upload `rust_deps_win_0.1.3.zip` to https://s3-us-west-2.amazonaws.com/rust-pkg-brave-core
Set the new version in `script/rust_deps_config.py` and create a pull request for your changes
```