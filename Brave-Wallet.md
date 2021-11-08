Brave Wallet was created as part of the Brave browser in order to bring cryptocurrency and web 3 access to Brave's users.

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


## HD Wallet

Bitcoin and Ethereum generate addresses in different ways, but they share the same key derivation implementations as BIP-32 compatible wallets. 

The Brave wallet implements BIP-32 (HD wallet), BIP-39 (Mnemonic keywords), BIP-43 (Multipurpose HD wallet structure), and BIP-44 (Multicurrency and multi account wallets).

The mnemonic words are encrypted with AES-GCM symmetric encryption and stored in preferences with the user passphrase. 

Which account paths are created / derived are stored in preferences as well. We will support discovery of them when restoring too via https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki#account-discovery

Bitcoin and Ethereum use an elliptic curve named secp256k1. We use Boring SSL in Brave, but that library does not support secp256k1.  Instead we use `libsecp256k1` from [Bitcoin Core](https://github.com/bitcoin/bitcoin). 


## Address formats

On Ethereum, both externally-owned accounts and contract accounts are 40-character hex encoded strings with a “0x” prefix.  0x + the last 20 bytes of the `keccak` hash of the public key.

For testing: http://bip32.org/

Bitcoin addresses are encoded using `bech32` base 58 encoding (base-58 encoding also from https://github.com/bitcoin/bitcoin).

https://en.bitcoin.it/wiki/BIP_0173#Segwit_address_format
https://en.bitcoin.it/wiki/Segregated_Witness

## Working with big numbers

It is common to work in very large numbers in Ethereum for both ETH and for tokens.  For example eth = 1,000,000,000,000,000,000 wei.  We use `unsigned _ExtInt(256)` type for working with these large numbers. For mojo interfaces which allow communication across processes and languages, we support a second format of a hex encoded string, e.g. “0xDE0B6B3A7640000”.  There are conversion functions to and from the hex string format and the `uint256_t` function.

## Hashing in Ethereum

Hashing in Ethereum uses something close to SHA3 with slightly different parameters than SHA3. In particular it uses `Keccak F1600` (ether version).  Since we don’t have the ability to get the keccak hash in Chromium, we’ve added this third party dependency:
https://github.com/chfast/ethash.git
Apache License, Version 2.0.

For reference:
```
Keccak256("") =
c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470

SHA3("") =
a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a 
```
