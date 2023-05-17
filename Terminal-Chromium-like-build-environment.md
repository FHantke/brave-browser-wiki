To run `gn`, `ninja`, `goma_ctl` and other well-known Chromium tools directly in the current terminal session use helpers located in `brave/build` directory:

```
# cygwin/linux/macos
. brave/build/env.sh

# windows powershell
. brave\build\env.ps1

# windows cmd.exe
brave\build\env.cmd
```

Pass `-v` to see what variables are set.