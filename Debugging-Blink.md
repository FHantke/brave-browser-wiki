#### Child process auto-attach
To debug Blink and other child processes, a debugger should support child process auto-attach.

There are some extensions to make this happen:

* `Visual Studio 2022` https://marketplace.visualstudio.com/items?itemName=vsdbgplat.MicrosoftChildProcessDebuggingPowerTool2022
* `Visual Studio Code` https://github.com/albertziegenhagel/childdebugger-vscode

#### Disable pointer compression

V8 and Blink uses [pointer compression](https://v8.dev/blog/pointer-compression) by default. This lowers RAM usage, but prevents debuggers to display any members properly.

To disable pointers compression add this to `out/<Config>/gn.args`:

```
v8_enable_pointer_compression = false
cppgc_enable_pointer_compression = false
cppgc_enable_caged_heap = false
```