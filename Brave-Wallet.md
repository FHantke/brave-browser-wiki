Brave Wallet was created as part of the Brave browser in order to bring cryptocurrency and web 3 access to Brave's users. Brave Wallet is built for Desktop, Android (coming soon), and iOS (coming soon).

Users can access the Brave Wallet by navigating to brave://wallet or from the wallet icon at the top of the browser.

The Brave wallet will be home to a user's local cryptocurrency wallet, Brave Rewards wallet, credit cards, and other exchanges that we support oauth’ing to.

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

## Hardware wallets

We support both Ledger and Trezor wallets. 

Ledger will works through the ledger JS bridge.  Which means code for hardware wallet can only run when the page or panel is open.  When a user first sets up a hardware wallet it will store the metadata about the hardware wallet in preferences. A user will be prompted to connect their hardware wallet when they want to send a transaction with an address that belongs to the hardware wallet or sign data.

## Updatable data files

Some data that the Brave Wallet uses is retrieved at run time from the component update server so that it can be updated out of line from browser release updates. This component is named "Brave Wallet data files" and you can see it in brave://components/

- **Name: Contract metadata**  
Source: https://github.com/MetaMask/contract-metadata/blob/master/contract-map.json and coinmarketcap data for alternate chains like BSC.  
Purpose: To list assets that a user can watch and interact with

- **Name: List of network chains**  
Source: Subset of https://chainid.network/chains.json  
Purpose: For working with EIP-3085

- **Name: List of popular Dapps**  
Source: We’ll create a customized list of known Dapps  
Purpose: For adding a browse popular Dapps feature

Desktop, Android and iOS will have the same component ID `bbckkcdiepaecefgfnibemejliemjnio`.

Updates will occur as part of the normal component update flow in Brave described here: https://github.com/brave/brave-browser/wiki/Component-Extensions

The component is downloaded into the user's profile directory in a subdirectory named `BraveWallet/`.


## Transactions

If an account has given permissions for an address to a page. That page can request the user to sign a transaction.  The Brave Wallet will try to deduce what the transaction does to inform the user.  The details of the transaction will also be shown.  Transaction approval will always happen from the Brave Wallet panel. Transactions are initiated by pages that use the `eth_sendTransaction` method in a `window.ethereum.request` call.

Transaction signatures use the selected network’s chain ID to avoid replay protection.

## Signing data

If an account has given permissions for an address to a page. That page can request that the account sign a message. 

There will be a number of signing methods exposed, currently the supported methods are:
`eth_sign` and `personal_sign`

## Adding and switching to other EVM compatible chains

EIP 3085 describes `wallet_addEthereumChain` in https://eips.ethereum.org/EIPS/eip-3085 for adding EVM compatible chains.
EIP 3326 describes `wallet_switchEthereumChain` in https://eips.ethereum.org/EIPS/eip-3326 for switching to EVM compatible chains.

Some EVM compatible chain examples include Binance Smart Chain, L2s such as Polygon, Arbitrum, Optimism, and side chains such as SKALE, xDAI.  Not all of these chains need to have a native currency of ETH, for example Binance Smart Chain has a native symbol of BNB and Polygon has MATIC.

We may preload some chains and L2s to avoid phishing attempts from a page wanting to add another network. 

A page can specify the following fields: 
`NetworkName`, `NetworkURL`, `ChainID`, `CurrencySymbol`

A list of known EVM networks can be found here: https://chainid.network/ we’ll make a subset of this list as our officially supported list.
We’ll give a UI warning if a page is requesting something to be added that’s not in that list. 

The list of added networks can also be managed in preferences.

## Upgrading to the Brave Wallet

Users of Crypto Wallets and MetaMask have the ability to import their HD wallets to Brave Wallet.
On first setup of the Brave Wallet, a user will be given the option to automatically import from MetaMask or from Crypto Wallets if Brave detects it is installed.  If the user selects to import, the browser will ask for their password and use that to decode their seed words. The password never leaves the user's device and is not kept in memory.

Users can still use Crypto Wallets by changing their default wallet in settings. 

Users can still use MetaMask as the Brave Wallet does not have any special handling for it and allows it to overwrite `window.ethereum` by default.  This can be changed in brave://settings/wallet.

A user can also later import accounts from a hex string or JSON format.  When restoring Crypto Wallets from seed words, we support both the legacy derivation paths that are specific to Crypto wallets and the new BIP39 compatible ones.  The user will be given the option to select which one if they enter 24 words.  If a user doesn't know which version they are on, and a user picks the wrong way and doesn't see their balance, they can try again with the other option.

## Brave developer information

For information for Brave wallet developers, please see https://github.com/brave/brave-browser/wiki/Brave-Wallet-developer-information

