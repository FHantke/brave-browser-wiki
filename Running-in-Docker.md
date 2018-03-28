You can compile Brave for Linux using a Docker container.

First clone this repo and enter the repo directory:
```
git clone https://github.com/brave/brave-browser.git
cd brave-browser
```

Start Docker then build the image from the Dockerfile:
```
docker build -t blb .
```

Now run the image interactively, mounting the appropriate directories:
```
docker run --rm -it -v $(pwd):/src -v $(pwd)/.sccache:/root/.cache/sccache blb
```

After starting the container, perform the `yarn install` and `yarn run init` steps from the [[standard build instructions|Home]].
 
Then, install the Chromium dependencies by following [[the Linux Development Environment setup process here|Linux-Development-Environment]].

Lastly you can proceed with [[remaining build scripts|Home#build-brave]] such as `yarn sync --all` and `yarn build --debug_build=true --official_build=false`. See `./src/out` for the results.  For incremental builds you can also pass `--no_branding_update` for a faster build.
