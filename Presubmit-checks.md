Chromium has a system of presubmit checks which contains a lot of Chromium-specific validators we can use. The system is greatly explained here: https://www.chromium.org/developers/how-tos/depottools/presubmit-scripts/

## Presubmit checks in Brave
To run presubmits in Brave: `npm run presubmit`, the result will be printed to the console.

There're few options you may find useful:
```
--base <base branch>  set the destination branch for the PR
--all                 run presubmit on all files
--files <file list>   semicolon-separated list of files to run presubmit on
--verbose [arg]       pass --verbose 2 for more debugging info
--fix                 try to fix found issues automatically
```

If you need to configure some upstream checks, i.e. disable or add a file filter, please use:
[chromium_presubmit_config.json5](https://github.com/brave/brave-core/blob/master/chromium_presubmit_config.json5).

## Available checks
#### Brave checks
* license header check

#### Chromium checks
In addition to manually written checks, the presubmit system also runs:
* code format (clang-format, yapf)
* cpplint
* pylint
* checkdeps
* eslint

It can be extended to support other checks, such as `chromium_src` macro validation.

The system also supports Android style check, but it's currently [disabled via config file](https://github.com/brave/brave-core/blob/3f8492e4be2b5adfab8ddb65fd49e7412d019710/chromium_presubmit_config.json5#L73-L74), because most `.java` files require style rework.

## Chromium modifications
Currently upstream checks are modified in such way:
1. Make it compatible with our directory structure.
2. Disable Gerrit-specific checks.
3. Disable `OWNERS`/`AUTHORS` checks.
4. Add ability to fully disable a check or filter some files.
5. Force some checks to trigger errors instead of warnings.

Under the hood we run `git cl presubmit` in `src/brave` directory, which runs all checks from `src/brave/**/PRESUBMIT.py` and upstream checks from `src/PRESUBMIT.py`.

`src/PRESUBMIT.py` is modified using [chromium_presubmit_overrides.py](https://github.com/brave/brave-core/blob/master/script/chromium_presubmit_overrides.py) to support `chromium_presubmit_config.json5`.