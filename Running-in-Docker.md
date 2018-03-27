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

Dependencies are included in the image so there's no need to run `./src/build/install-build-deps.sh`.

Now you can proceed with build scripts such as `yarn install`, `yarn run init`, `yarn sync --all` and `yarn build --debug_build=true --official_build=false`. See `./src/out` for the results.  For incremental builds you can also pass `--no_branding_update` for a faster build.
