One of the primary goals of brave-core is to do changes in a way that makes it easy for Chromium rebases. This is because speed of updating to a new Chromium version is important to us. 

Please follow this order when doing patching from best to worst:

## Changes only inside brave-core

If changes can be made inside existing subclasses and code inside `src/brave`, then that is preferred.

## Subclass and override

When you can't make a change directly in existing brave-core code, it's often best to simply subclass a Chromium class, and override the functions needed.

You will need to patch (see the below documentation) for some small trivial things in this case:
- Create instances of your class instead of the Chromium class.
- Possibly add a `friend` member to the base class you're subclassing.
- Possibly add a `virtual` keyword to functions you'd like to subclass.

## Using the preprocessor to use base implementations inside override files

One strategy that's preferred over patching is to use `src/brave/chromium_src` which overrides `.cc` and `.h` files but still use the source in the original Chromium code too.  To do that you can rename a function with the preprocessor in Chromium, and then provide your own real implementation of that file and use the Chromium implementation inside of it.

Here's an example:
https://github.com/brave/brave-core/blob/5293f0cab08816819bb307d02e404c2061e4368d/chromium_src/chrome/browser/browser_about_handler.cc

No BUILD.gn changes are needed for this.

## Override a .cc file completely

If you want to provide a completely different implementation of a file, it is often not safe, but sometimes applicable. You can just provide the alternate implementation inside the `src/brave/chromium_src` directory.

One way electron went wrong is they copied entire files for changes inside a similar setup, do NOT do this. This will lead to newer Chromium rebases over time using old stale code which causes problems and makes rebasing much harder.

No BUILD.gn changes are needed for this.

## Patch the Chromium files

When other options are exhausted, you can patch the code directly in `src/`. After making the changes, you can run the npm command `npm run update_patches`.   This will update the patches which are stored in  `src/brave/patches`.   Please note that removed changes in `src` currently will not update the patches, so you will have to do that manually. 

We aim to make the only patches required to be trivial changes, and not nested logic changes.

Patch changes which affect the flow of execution should ideally be wrapped in `#if defined(BRAVE_CHROMIUM_BUILD)` and `#endif` blocks.

You should almost never patch in two methods calls in a row. We should prefer extensible patches. For instance https://github.com/brave/brave-core/pull/2693/files#diff-a9c9a8da7aa4df821394352a0ca04a27R12:
```
CopyBraveExtensionLocalization(config, staging_dir, g_archive_inputs)
CopyBraveRewardsExtensionLocalization(config, staging_dir, g_archive_inputs)
```
inside `CopyAllFilesToStagingDir` would be collapsed to
```
CopyBraveFilesToStagingDir
```

Make sure you do NOT have the following in your `~/.gitconfig`:

    [apply]
            whitespace = fix

as trailing whitespace can be essential in patch files.

## Patching gn/gni files
gn patches should 