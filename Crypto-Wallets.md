# Crypto Wallets

## Install of Crypto Wallets on first use

A user can install Crypto Wallets by either navigating to `brave://wallet`, or by going to a Dapp and confirming to install via the infobar that comes up.

## Dapp detection

Before Crypto Wallets is installed, the browser does something called Dapp detection.
This is the process of injecting a content script to see if window.web3 is accessed without actually injecting web3.

If window.web3 is accessed, then we show an infobar to install Crypto Wallets.  If MetaMask is already installed, we prompt the user to see if they'd like to use Crypto Wallets or MetaMask as their Dapp provider.


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