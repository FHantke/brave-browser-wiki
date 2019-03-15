## Introduction
In the `brave-core` repo, there is a script you can use to find regressions on Desktop. It will fetch releases from our GitHub page and download/install them. Based on the user input (does this work?) a binary search is done to narrow down possible builds until the actual bad release is found. At that point, the commit log for that version is shown

For Brave employees, an overview of the tool was given and recorded which can be found here:
https://drive.google.com/a/brave.com/file/d/16RYK-yHj1A9S4iLhWiorn6py6jMbpjPY/view?usp=sharing

## Getting started
There's a Python 2 script available in brave-core which helps automate the tedious task of creating all the pull requests:
`./script/build-bisect.py`

In order to use this, you'll need to create a GitHub personal access token
- visit https://github.com/settings/tokens
- click `Generate new token`
- set the scopes (if needed). If you're not sure what to do, you can always change them later
- capture the token that is output
- open your .npmrc (`~/.npmrc`) and add the token value (NOTE: if you do this and store your dotfiles publicly (on GitHub/GitLab/etc), you'll want to specifically add `.npmrc` to your `.gitignore` file)
    ```
    BRAVE_GITHUB_TOKEN=0a0aaa00a0000aa0aaa0000000a0a0000a00aa0a
    ```

If you don't want to store this in your `.npmrc`, you can always set the environment variable or include the environment variable in the usage:
```
BRAVE_GITHUB_TOKEN=0a0aaa00a0000aa0aaa0000000a0a0000a00aa0a ./script/uplift.py --options-go-here
```

## Example usage
### Search all versions
- With no parameters, all releases will be fetched
    `./script/build-bisect.py`

### Provide good/bad versions
- If you know good and/or bad versions, you can provide those with the optional parameters:
    `./script/build-bisect.py --good=0.60.0 --bad=0.61.36`

    For example, in the above- 0.60.0 isn't a real version, but it'll match the first 0.60.x release. This is intended to be a version where the feature WAS working. `--good` and `--bad` are both optional; if you only know one of them, that's OK!

## Other flags
- To get more info (debug logs), use the `--verbose` parameter 
- Restrict bisect to a particular channel with `--channel`. ex: `--channel=dev`, `--channel=beta`, `--channel=release`
- If for some reason you wanted to use your actual profile directories (instead of the temp ones), you can use the `--real-profile` parameter
- You can provide a zip file that will get downloaded for each version and unzipped into the temporary profile directory. For example, I have a 5000 bookmarks profile which you can use to seed your profile. You would enable this via the following command line argument:
`--use-profile="https://github.com/brave/qa-resources/blob/bsc-profile/profiles/5000-bookmarks.zip?raw=true"`

## Notes / Limitations
- Temporary profile directories are created for each run. No need to worry about your real browser data being modified 😄 
- Only works for macOS at the moment (edits welcome for Windows and/or Linux)
- Tool could also be used to find first appearance of a feature. However, it's confusing because it prompts you asking if build is broke or not. When using for this purpose, you would just answer `y` for versions that DON'T have the feature and `n` for versions that DO.

## History
- Originally implemented with https://github.com/brave/brave-core/pull/1833