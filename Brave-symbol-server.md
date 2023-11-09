### What's uploaded and how to access

Official Windows Brave builds (arm64, x86, x64) upload PDBs and binary images to Brave symbol server.

The symbol server is accessible via https://brave-symbols.s3.brave.com, but it's not meant to be human-usable. The server should be configured via debugger, the debugger will download everything it needs according to the Microsoft symsrv structure.

### Configuring Visual Studio

To setup Visual Studio to use the symbol server you can either:
* set `_NT_SYMBOL_PATH` environment variable before running `devenv.exe`:
```
set _NT_SYMBOL_PATH=SRV\*C:\symbols\*https://msdl.microsoft.com/download/symbols;SRV\*C:\symbols\*https://brave-symbols.s3.brave.com
```
* or add `https://brave-symbols.s3.brave.com` via Options dialog:
![image](https://github.com/brave/brave-browser/assets/5928869/8bbd3c98-02d7-4b2b-b4cf-d23d04353377)

### Using automatic source file downloads

Brave PDBs include source server information which allows Visual Studio to automatically download source files from GitHub. This greatly simplifies the debug process as you don't need to have the exact Brave source tree checked-out locally.

To have this working you need to install Python 3 and make sure [`py` launcher](https://docs.python.org/3/using/windows.html#launcher) is in your `PATH`.

The download is triggered by Visual Studio when a call stack frame is opened. One time per PDB you will see a security dialog asking you to execute a command:

![image](https://github.com/brave/brave-browser/assets/5928869/b8f97b9a-f0e5-4cde-910b-ea42ed5698cf)

The command in this example look like this:
```
cmd /c "mkdir "C:\Users\user\AppData\Local\SOURCE~1\base\task\thread_pool\worker_thread.cc\84eb7c6e8a38b0e65b14f75cfec8144ffbc4b359" & py -3 -c "import urllib.request, base64;url = \"https://raw.githubusercontent.com/brave/chromium/84eb7c6e8a38b0e65b14f75cfec8144ffbc4b359/base/task/thread_pool/worker_thread.cc\";u = urllib.request.urlopen(url);open(r\"C:\Users\user\AppData\Local\SOURCE~1\base\task\thread_pool\worker_thread.cc\84eb7c6e8a38b0e65b14f75cfec8144ffbc4b359\worker_thread.cc\", \"wb\").write((u.read()))"
```

If you trust the command, then:
1. Choose "Trust commands from this symbol file"
2. Press "Run".

Visual Studio will download symbols and display the exact source location automatically.

### PGO and other optimizations notes

Brave is built with [Profile-guided optimizations](https://clang.llvm.org/docs/UsersManual.html#profile-guided-optimization) enabled, which may change the code flow substantially, and even having all symbols the execution during debugger steps might look weird. That's the sad reality of PGO builds.
