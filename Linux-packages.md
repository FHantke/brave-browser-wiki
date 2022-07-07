Here are instructions on how to build the [brave-browser Linux packages](https://brave-browser.readthedocs.io/en/latest/installing-brave.html#linux) during development.

# Normal user

If building in a virtual machine, make sure you don't build as `root` or the [`npm run init` might fail](https://github.com/brave/brave-browser/issues/2424).

# Build command

Once you've installed the [pre-requisites](https://github.com/brave/brave-browser/wiki/Linux-Development-Environment) and [cloned the repo](https://github.com/brave/brave-browser/wiki), the build command to use is:

    npm run create_dist -- Release

In particular, make sure you don't specify `--official_build=false` or your [build might fail](https://github.com/brave/brave-core/pull/1078#issuecomment-454969015) (see [#305](https://github.com/brave/brave-browser/issues/305)).

Once the build is done, you'll find the `.deb` and `.rpm` directly in `src/out/Release/`.

## Beta / dev packages

To build beta or dev packages, use the `--channel` argument:

    npm run create_dist -- Release --channel=beta