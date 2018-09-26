# System Requirements

Before you begin, ensure your system satisfies the [system requirements](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md#system-requirements).

# Install additional build dependencies

You will need Git, Python 2.7, NodeJS >= 7.x, and npm.

> Alternatively to npm, you can also use Yarn. We recommend following the [Yarn install docs](https://yarnpkg.com/lang/en/docs/install/) to install Yarn and a compatible version of NodeJS. After installing `yarn` you'll want to run `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

If you are using Ubuntu, additionally install:

```
apt-get install build-essential libgnome-keyring-dev python-setuptools rpm
```

You are now ready to [[clone and initialize the repo|Home#clone-and-initialize-the-repo]]. After `npm run init` is finished, there is one final step to finish installing build dependencies. This shell script only works on Debian and Ubuntu but check [system requirements](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md#system-requirements) for other distros: 

```
# cd to brave-browser repo root
./src/build/install-build-deps.sh
```
You might also want to try `./src/build/install-build-deps.sh --unsupported` if above command gives an error about using a non supported Linux distribution.

# Troubleshooting

Check out the upstream [Checking out and building Chromium on Linux](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md) docs before filing an issue.
