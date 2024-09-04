#### Child process auto-attach on Windows
To debug Blink and other child processes, a debugger should support child process auto-attach.

There are some extensions to make this happen on Windows:

* `Visual Studio 2022` https://marketplace.visualstudio.com/items?itemName=vsdbgplat.MicrosoftChildProcessDebuggingPowerTool2022
* `Visual Studio Code` https://github.com/albertziegenhagel/childdebugger-vscode

#### Disable pointer compression

V8 and Blink uses [pointer compression](https://v8.dev/blog/pointer-compression) by default. This lowers RAM usage dramatically, but prevents debuggers to display most variables properly.

To disable pointers compression add this to `out/<Config>/args.gn`:

```
v8_enable_pointer_compression = false
cppgc_enable_pointer_compression = false
cppgc_enable_caged_heap = false
```