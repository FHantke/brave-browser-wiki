# Auto bump DEPS

A Github Actions [workflow](https://github.com/brave/brave-core/actions/workflows/update-dep.yml) has been created that makes the process of auto bumping DEPS easier.

You can run it by visiting https://github.com/brave/brave-core/actions/workflows/update-dep.yml
and clicking the `Run workflow` button on the top-right.

You can also run it from cli by: `gh workflow run update-dep.yml [-f dep=vendor/web-discovery-project] [-f ref=commit-hash]`
If `ref` is not given, we will try to use the latest commmit (tip) of a branch called `master` or `main`
