# Crypto Wallets (Deprecated)

## Install of Crypto Wallets on first use

A user can install Crypto Wallets by either navigating to `brave://wallet`, or by going to a Dapp and confirming to install via the infobar that comes up.

## Dapp detection

Before Crypto Wallets is installed, the browser does something called Dapp detection.
This is the process of injecting a content script to see if window.web3 is accessed without actually injecting web3.

If `window.web3` is accessed, then we show an infobar to install Crypto Wallets.  If MetaMask is already installed, we prompt the user to see if they'd like to use Crypto Wallets or MetaMask as their Dapp provider.


## Settings

A number of settings are kept relating to Crypto Wallets.

Seed related keys: 
- `brave.wallet.aes_256_gcm_siv_nonce`
- `brave.wallet.encrypted_seed`

Deprecated settings:
- `brave.wallet.enabled` - It used to be used to actively disable Crypto Wallets.

Other settings:
- `brave.wallet.pref_version` - The version of the prefs the browser is running.
- `brave.wallet.web3_provider` - The web3 provider to use, it's an integer with values for: `Ask`, `None`, `Crypto Wallets`, and `MetaMask`.


## Selecting a web3 provider:

A web3 provider can be selected in `brave://settings` in the Extensions section.


<img width="626" alt="Screen Shot 2019-12-20 at 12 09 30 PM" src="https://user-images.githubusercontent.com/831718/71272482-9b9ef400-2321-11ea-8472-0e18e5c3cca9.png">

This setting indicates which (if any) web3 provider should be used. It has values for `Ask`, `None`, `Crypto Wallets`, and if it is installed, `MetaMask`.

The way this setting works is that it will actively disable the extension's content-script which injects `window.web3` for MetaMask and Crypto Wallets, and only enable content scripts for the extension that was selected.

## APIs

There are a number of [Brave extension APIs](https://github.com/brave/brave-core/blob/master/common/extensions/api/brave_wallet.json) that are exposed to the brave-extension (for Dapp detection), Ethereum Remote Client, and MetaMask.


## Testing and publishing new Crypto Wallets releases


### General info about the component extension

The Crypto Wallets Extension ID is: `odbfpeeihdkbihmopkbjmoonfanlbfcl`

You can see the list of extensions and versions that are being served via this endpoint:

- [Production Server](https://go-updater-dev.bravesoftware.com/extensions/test)
- [Dev Server](https://go-updater.brave.com/extensions/test)


### Pushing up an update

- Make sure all changes are committed to brave/ethereum-remote-client
- Run the Jenkins job `brave-ethereum-remote-client-build`
- In the `publish` phase it will ask for OTP, get it from 1password in the Developers Shared vault under "npm - devops"
- Update the version pinned in brave-core-crx-packager [here](https://github.com/brave/brave-core-crx-packager/blob/master/package.json#L11) and in the lock file. 
- Commit and merge that change :)
- Run the Jenkins job `brave-core-ext-ethereum-remote-client-update-publish-dev`

There is no need to manually purge the dev updater cache, it does so every 5 - 15 minutes. 

Send a message to #wallet something like this:

```
Crypto Wallets release candidate is ready for testing
When it goes to the prod server it will be 1.0.28
On the dev server it is 1.0.26
Release tag is: https://github.com/brave/ethereum-remote-client/releases/tag/0.1.89
Milestone: https://github.com/brave/brave-browser/milestone/182?closed=1
```

### Testing

It's a good idea to check the version number before and after you start QA'ing to make sure the new one is available. 
Please note that the automatic version of extensions generated on the development server is different from the production server. 

To QA new Crypto Wallets releases use the development component updater with a fresh profile.  

To do this you can use the command line argument `--use-dev-goupdater-url`.

You can use a clean profile without clearing with this as well: `--user-data-dir=<tmp-dir>`.

If you're using a development build, you can set the dev server via this npmrc environment in ~/.npmrc:

`updater_dev_endpoint=https://go-updater-dev.bravesoftware.com/extensions` 

You can test a new profile using the dev component with something like this:   
`open -a Brave\ Browser\ Beta.app --args --use-dev-goupdater-url --user-data-dir=$(mktemp -d)`

### Sign off

After QA gives sign off in Slack on #releases

- Run the `brave-core-ext-ethereum-remote-client-update-publish` Jenkins job.
- The production updater self purges its cache every 5 - 10 minutes.

