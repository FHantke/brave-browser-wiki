Taken from https://github.com/brave/browser-android-tabs/wiki/Symbolizing-Crash-Stacks-on-Play-store

### Brief description

If you have a stack trace that needs to be symbolized, copy it into a text file(dump file) and symbolize with the following command +
`third_party/android_platform/development/scripts/stack --output-directory out/DefaultR [dump file]`

### Detailed instruction 

*Based 1.6.2 release report as an example*

*(preparation part)*

0) Checkout repo on a proper point according to version.

1) Download symbols for arm and arm64:
DefaultRArmMono.tar.7z
DefaultRArm64Mono.tar.7z

*Maybe you will need to download some others, but these are most often currently*

2) Unpack them for example into 
`~/Projects/andro_master/symbols/src/out/DefaultR` and 
`~/Projects/andro_master/symbols/src/out/DefaultR64`

3) Arm64 apks now contain native libs both for arm and arm64, but code is loaded from arm even on arm64 devices. So for arm64 a workaround below is required. If you know your crash is from arm, you can skip it.

3.1 rename `symbols/src/out/DefaultR64/libmonochrome.so` => `symbols/src/out/DefaultR64/libmonochrome.so_BAK`

3.2 copy `symbols/src/out/DefaultR64/android_clang_arm/libmonochrome.so` => `symbols/src/out/DefaultR64/libmonochrome.so`

3.3 rename `symbols/src/out/DefaultR64/lib.unstripped` => `symbols/src/out/DefaultR64/lib.unstripped_BAK`

3.4 copy `symbols/src/out/DefaultR64/android_clang_arm/lib.unstripped` => `symbols/src/out/DefaultR64/lib.unstripped`

*(main part)*

4) Take the crash stack form GP console
```
app version 394510002
backtrace:
  #00  pc 0000000001501988  /data/app/com.brave.browser-ySbcdux4VH_TH32LDOYCxg==/base.apk (offset 0xb7e000)
```
Save it to the file `browser-android-tabs/src/StackGP1.txt`

5) You need to understand what the version is this.
Open `symbols/src/out/DefaultR64/android_chrome_versions.txt`, `symbols/src/out/DefaultR/android_chrome_versions.txt` and find what is `394510002` :
it is `Monochrome: 394510002` at `DefaultR`

6) Run 
`./third_party/android_platform/development/scripts/stack --output-directory /mnt/EVO970/Projects/andro_master/symbols/src/out/DefaultR --verbose --more-info   ./StackGP1.txt`

*note, we are using `--output-directory` with path for arm mono according to pt5.*

and get the result  
```
Stack Trace:
  RELADDR   FUNCTION                                       FILE:LINE
  0000000001501988  bat_ledger::LedgerImpl::OnTimer(unsigned int)  ../../brave/vendor/bat-native-ledger/src/bat/ledger/internal/ledger_impl.cc:717:27
```
6.1 If you don't see a truthful stack and there is a message 
```
Can't detect shared library in APK, fallback to library libchrome.so
```
you can try to rename at your out symbols dir   
```
libmonochrome.so => libchrome.so
lib.unstripped/libmonochrome.so => lib.unstripped/libchrome.so
lib.unstripped/libmonochrome__combined.so => lib.unstripped/libmonochrome__combined.so
lib.unstripped/chrome__combined.so.map.gz => lib.unstripped/chrome__combined.so.map.gz
```
and try to run script again
 