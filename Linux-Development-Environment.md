## System Requirements

Before you begin, ensure your system satisfies the [system requirements](https://chromium.googlesource.com/chromium/src/+/lkcr/docs/linux_build_instructions.md#system-requirements).

## Install additional build dependencies

You will need Git, Python 2.7, NodeJS >= 7.x, and Yarn. We recommend following the [Yarn install docs](https://yarnpkg.com/lang/en/docs/install/) to install Yarn and a compatible version of NodeJS.

If you are using Ubuntu, additionally install:

```
apt-get install build-essential libgnome-keyring-dev python-setuptools rpm
```

You are now ready to [[Clone and initialize the repo|wiki#clone-and-initialize-the-repo]]. After `yarn run init` there is one final step to finish installing additional build dependencies:

```
# cd to brave-browser repo root
./src/build/install-build-deps.sh
```

## Troubleshooting

Check out the upstream [Checking out and building Chromium on Linux](https://chromium.googlesource.com/chromium/src/+/lkcr/docs/linux_build_instructions.md) docs before filing an issue.