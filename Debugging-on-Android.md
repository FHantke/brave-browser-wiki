If your build crashes, you can get a symbolized stack trace of the last crash via:

`build/android/tombstones.py --output-directory out/android_Debug_x86/`


## Unittests


### Attaching debugger

I haven't gotten this to work but as I understand, this should work.
I'm putting as far as I got so far in case someone can take it further.

`/out/android_Debug_x86/bin/run_brave_unit_tests --wait-for-debugger -t 6000`
`build/android/adb_gdb --output-directory=out/android_Debug_x86 --package-name=org.chromium.native_test --verbose --target-arch=x86`

At the gdb prompt, type `c` to continue