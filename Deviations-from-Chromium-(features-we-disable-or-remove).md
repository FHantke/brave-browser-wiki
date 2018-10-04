Brave for desktop is built on top of the [open-source Chromium project](https://www.chromium.org/chromium-projects). We add features on top of what is already there and we also remove features or pieces of the code. These deviations we make that touch the core Chromium code are done via patching

## How it works
If you wanted to do an audit of the code, you would start with the [`brave-browser`](https://github.com/brave/brave-browser) repository. [Our wiki has instructions](https://github.com/brave/brave-browser/wiki) about what steps need to be done to perform a build after cloning the source

### Chromium source is fetched
The gclient utility (part of depot tools) will fetch the official Chromium source code. The tag that is fetched is [captured in our package.json](https://github.com/brave/brave-browser/blob/master/package.json) (for example, `70.0.3538.35`). All of the source code will be downloaded into the `./src/` folder

### Brave code is fetched
As part of the setup process, we also fetch our own code. The [`brave-core` repository](https://github.com/brave/brave-core) has the code that makes the browser Brave. The branch that should be checked out is also contained in that [package.json](https://github.com/brave/brave-browser/blob/master/package.json). There is also a [`DEPS` file in `brave-core`](https://github.com/brave/brave-core/blob/master/DEPS) that pulls in sub-dependencies ([such as the `brave-extension`](https://github.com/brave/brave-extension))

### Hooks are ran
After the gclient sync runs and fetches all the code (including `brave-core`), the hooks are ran. One of the hooks that runs applies the patches ([which you can see here](https://github.com/brave/brave-core/tree/master/patches)) that are contained in `brave-core`. If you'd like to know more details about HOW the patching works, you can [take a peek at our patching wiki page](https://github.com/brave/brave-browser/wiki/Patching-Chromium)

## What kind of patches are in place?

..TODO..