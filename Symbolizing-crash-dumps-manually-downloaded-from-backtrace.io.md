It may happen that you need to get symbolized crash stack from non-official build from the backtrace.io.

Your steps to resolve such case are below:

_The commands below are from arm64 mono build. If you use another build, change the folders names correspondingly_

1. Download the minidump file from backtrace.io crash via `Debug => Attachments => (raw) => Download`

2. Build dump tools:
```
#BuildDumpTools.sh

cd brave-browser/src
ninja -C out/android_Release_arm64 minidump_stackwalk dump_syms minidump_dump
```

3. Check whether you have `brave-browser/src/out/android_Release_arm64/dist/brave.breakpad.syms` dir, if you already have, skip pt3

4. Generate symbols:
```
#GenerateBreakpadSymbols.sh

md ./brave-browser/src/out/android_Release_arm64/dist/brave.breakpad.syms

./brave-browser/src/components/crash/content/tools/generate_breakpad_symbols.py             \
--build-dir=./brave-browser/src/out/android_Release_arm64                                   \
--symbols-dir=./brave-browser/src/out/android_Release_arm64/dist/brave.breakpad.syms        \
--binary=./brave-browser/src/out/android_Release_arm64/lib.unstripped/libmonochrome.so      \
--platform=android                                                                          \
--verbose
```

5. Run script to analyze the dump, in example below `./2cd18bf.dmp` is the path to downloaded file from pt1
```
#DecodeMinidump.sh

./brave-browser/src/out/android_Release_arm64/minidump_stackwalk  \ 
./2cd18bf.dmp  \
./brave-browser/src/out/android_Release_arm64/dist/brave.breakpad.syms 
```
