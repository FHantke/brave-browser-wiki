# Pushing up an update

Make sure all changes are committed to brave/brave-core-crx-packager which updates to the version of go-ipfs we want to use.
Run the Jenkins job `brave-core-ext-ipfs-component-publish-dev`

Send a message to Sri/Kamil, something like this:

go-ipfs is ready for testing
When it goes to the prod server it will be 1.0.4
On the dev server it is 1.0.5



# Testing

It's a good idea to check the version number before and after you start QA'ing to make sure the new one is available. Please note that the automatic version of extensions generated on the development server is different from the production server.

To QA new releases use the development component updater with a fresh profile.

To do this you can use the command line argument --use-dev-goupdater-url.

You can use a clean profile without clearing with this as well: --user-data-dir=<tmp-dir>.

If you're using a development build, you can set the dev server via this npmrc environment in ~/.npmrc:

updater_dev_endpoint=https://go-updater-dev.bravesoftware.com/extensions

You can test a new profile using the dev component with something like this:
open -a Brave\ Browser\ Beta.app --args --use-dev-goupdater-url --user-data-dir=$(mktemp -d)

# Sign off

After QA gives sign off in Slack on #releases

Run the `brave-core-ext-ipfs-component-publish` Jenkins job.
