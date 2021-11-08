Brave Wallet was created as part of the Brave browser in order to bring cryptocurrency and web 3 access to Brave's users.

Users can access the Brave Wallet by navigating to brave://wallet or from the wallet icon at the top of the browser.

The Brave wallet will be home to a user's local cryptocurrency wallet, Brave Rewards wallet, credit cards, and other exchanges that we support oauth’ing to such as Uphold, Binance, and Gemini or our own Creators portal (so that creators can manage their earnings balance through Brave Wallet)

## Ethereum provider global object

The `window.ethereum` object is provided by Brave's wallet to web pages. You can read about it here: https://github.com/brave/brave-browser/wiki/Ethereum-Provider-API

## Settings

Brave Wallet settings can be accessed from: brave://settings/wallet

#### Default wallet

The default wallet setting can be set to one of the following values:

- `Brave Wallet (Prefer extensions)`: This is the default setting. With this setting, `window.ethereum` is exposed by Brave Wallet; however, extensions such as MetaMask are still allowed to overwrite `window.ethereum`.
- `Brave Wallet`: With this setting `window.ethereum` is exposed by Brave Wallet and extensions do not have access to overwrite `window.ethereum`.
- `Crypto Wallets (Deprecated)`: This setting only shows up for existing users that have used Crypto Wallets in the past.  Crypto Wallets was Brave's old cryptocurrency wallet and it was a MetaMask fork. If this setting is selected, navigating to brave://wallet will load Crypto Wallets instead of Brave Wallet. New users can access this setting only if they have brave://flags/#ethereum_remote-client_new-installs set to true manually.
- `None`: With this setting users can still navigate to brave://wallet, however no `window.ethereum` object is exposed.

### Default base currency

The default base currency allows you to select which currency you'd like to display asset prices in. The default is USD.

### Default base cryptocurrency

The default base cryptocurrency allows you to select which cryptocurrency you'd like to display asset prices in. The default is BTC.

### Show Brave Wallet icon on toolbar (Desktop only)

This controls wether there is a wallet icon at the top of your browser or not on Desktop.  If there is no icon and there are Dapp requests, they will popup from the hamburger menu.

### Automatically lock Brave Wallet

The number of minutes to wait until the Brave Wallet is automatically locked. The default is 5 minutes.

### Networks

EVM compatible networks can be added here: brave://settings/wallet/networks
You can visit https://chainlist.org/ for a list of supported chains.

### Reset Wallet

This allows you to reset all of your wallet state back to the original default. Ensure you have a backup of your seed phrase and imported keys before using this option.

### Ethereum permission management

Permissions in Brave Wallet are managed by content settings. They can be modified for which sites have access to your addresses here:
brave://settings/content/ethereum

If a Dapp has previously requested access to a page and was given permission, then it will show up in this page as well.


## HD Wallet

Bitcoin and Ethereum generate addresses in different ways, but they share the same key derivation implementations as BIP-32 compatible wallets. 

The Brave wallet implements BIP-32 (HD wallet), BIP-39 (Mnemonic keywords), BIP-43 (Multipurpose HD wallet structure), and BIP-44 (Multicurrency and multi account wallets).
