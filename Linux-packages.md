Here are instructions on how to build the [brave-browser Linux packages](https://brave-browser.readthedocs.io/en/latest/installing-brave.html#linux) during development.

# Distro version

The official packages are currently built on Ubuntu 14.04, but they have also been built successfully on 16.04 and 18.04.

If using 14.04, you'll need to add a [third-party repo](https://launchpad.net/~openjdk-r/+archive/ubuntu/ppa) and install the `openjdk-8-jre` package.

# Normal user

If building in a virtual machine, make sure you don't build as `root` or the [`npm run init` might fail](https://github.com/brave/brave-browser/issues/2424).

# Build command

Once you've installed the [pre-requisites](https://github.com/brave/brave-browser/wiki/Linux-Development-Environment) and [cloned the repo](https://github.com/brave/brave-browser/wiki), the build command to use is:

    npm run create_dist Release -- --debug_build=true

In particular, make sure you don't specify `--official_build=false` or your [build might fail](https://github.com/brave/brave-core/pull/1078#issuecomment-454969015).

The `--debug_build=true` argument is optional but should speed up the build.

Once the build is done, you'll find the `.deb` and `.rpm` directly in `src/out/Release/`.