#### Child Process Auto-Attach on Windows

To debug Blink and other child processes, your debugger needs to support child process auto-attach.

On Windows, you can use these tools to make it work:

- **Visual Studio 2022**: [Child Process Debugging Power Tool](https://marketplace.visualstudio.com/items?itemName=vsdbgplat.MicrosoftChildProcessDebuggingPowerTool2022)
- **Visual Studio Code**: [Child Process Debugger Extension](https://github.com/albertziegenhagel/childdebugger-vscode)

#### Disabling Pointer Compression

V8 and Blink use [pointer compression](https://v8.dev/blog/pointer-compression) by default. This helps save RAM but can mess up how debuggers display most variables.

To turn off pointer compression, add this to your `out/<Config>/args.gn` file:

```gn
v8_enable_pointer_compression = false
cppgc_enable_pointer_compression = false
cppgc_enable_caged_heap = false
```
