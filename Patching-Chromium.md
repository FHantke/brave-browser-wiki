One of the primary goals of brave-core is to do changes in a way that makes it easy for Chromium rebases. This is because speed of updating to a new Chromium version is important to us. 

Please follow this order when doing patching from best to worst:

## Changes only inside brave-core

If changes can be made inside existing subclasses and code inside `src/brave`, then that is preferred.

## Subclass and override

When you can't make a change directly in existing brave-core code, it's often best to simply override a Chromium class, possibly add a `friend` member to your base class for your subclass.  Change the instance creation to use your class, and provide an override for the function. If the function is not `virtual`, you can patch to make it virtual. 

## Using the preprocessor to use base implementations inside override files

one strategy that's preferred over patching is to use `src/brave/chromium_src` which overrides `.cc` files but still use the source in the original Chromium code too.  To do that you can rename a function with the preprocessor in Chromium, and then provide your own real implementation of that file and use the Chromium one inside of it.

Here's an example:
https://github.com/brave/brave-core/blob/5293f0cab08816819bb307d02e404c2061e4368d/chromium_src/chrome/browser/browser_about_handler.cc

No BUILD.gn changes are needed for this.

## Override a .cc file completely

If you want to provide a completely different implementation of a file, it is often not safe, but sometimes applicable. You can just provide the alternate implementation inside the `src/brave/chromium_src` directory.

One way electron went wrong is they copied entire files for changes inside a similar setup, do NOT do this. This will cause Chromium rebases to get stale and cause problems. 

No BUILD.gn changes are needed for this.

## Patch the Chromium files

When other options are exhausted, you can patch the code directly in `src/`. After making the changes, you can run the npm command `npm run update_patches`.   This will update the patches which are stored in  `src/brave/patches`.   Please note that removed changes in `src` currently will not update the patches, so you will have to do that manually. 

We aim to make the only patches required to be trivial changes, and not nested logic changes.