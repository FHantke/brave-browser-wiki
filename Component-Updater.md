# What it does 

Brave's component updater server is an implementation of the Chromium component update server. The Brave browser will first try to ask our component updater for information about an extension. The component update server will will either return information directly to the Brave browser for our own extensions and components, or redirect the client so that the client requests the component from Chrome's component update server.

We have both a dev component updater and a prod component updater.

# Deployed instances

You can see a list of the components it serves by going to these URLs:
- Prod: https://go-updater.brave.com/extensions/test
- Dev: https://go-updater-dev.bravesoftware.com/extensions/test

The code for both of the above servers is from https://github.com/brave/go-update

# How it works

The go-update server works off of these 2 things:
A manifest of all the extensions and their current versions in MongoDB
S3 storage for the actual extension files.

# How we get things on the component update server

We both upload components to s3 and update manifests from the brave-core-crx-packager repo:
https://github.com/brave/brave-core-crx-packager

The component updater packages files first into an extension format. It does that in scripts similar to `scripts/packageComponent.js`.  Some components have their own packaging script, such as `scripts/packageTorClient.js`.

# Component versions

Versions are usually automatically generated using `util.getNextVersion`. Since dev and prod update servers are completely independent instances, with separate Mongo manifests. And each time we upload to one of them, we automatically get a new version, they have different version numbers.

There is no reason why our packaging script needs to be using an automatic version though.  We could for example add a command line argument to packageComponent to be able to specify a specific version to use.  Keep in mind though that we'd have to update Jenkins jobs accordingly if we did that.

# Testing

To QA components use the development component updater with a fresh profile.

To do this, you can use the command line argument `--use-dev-goupdater-url`.

You can use a clean profile without clearing with this as well: `--user-data-dir=<tmp-dir>`.

If you're using a development build, you can set the dev server via this npmrc environment in ~/.npmrc:

`updater_dev_endpoint=https://go-updater-dev.bravesoftware.com/extensions`