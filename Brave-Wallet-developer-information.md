The following information is mostly useful to Brave developers. 

Please see https://github.com/brave/brave-browser/wiki/Brave-Wallet for general information about the Brave Wallet.

Please see https://github.com/brave/brave-browser/wiki/Ethereum-Provider-API for information about the global Ethereum provider object.


## Architecture

The Brave Wallet is natively implemented as part of [brave-core](https://github.com/brave/brave-core).
Front ends of the Brave Wallet include the Brave desktop WebUI page and panel, Android native UI, and iOS native UI. 
These front ends communicate with the Brave Wallet through the [Brave Wallet mojo interface](https://github.com/brave/brave-core/blob/master/components/brave_wallet/common/brave_wallet.mojom).

Most front end functionality is accomplished through one of the following singleton controllers:
- `KeyringController`: Implements the HD wallet, Ledger & Trezor integration, account management, and signing.
- `EthJsonRpcController`: Deals with the ETH JSON RPC API, as well as things like the user's current network
- `SwapController`: Implements swapping related functionality through the 0x API
- `AssetRatioController`: Used for cryptocurrency prices over time
- `ERCTokenRegistry`: List of tokens and coins that are supported (This is likely to get renamed in the future).
- `EthTxController`: For transaction management such as creation, broadcasting, and storing transactions.
- `BraveWalletService`: For basically everything else including the default wallet setting, user assets, permission checks, importing functionality and much more.

Each controller typically also has an `Observer` named after it which allows the front end to update dynamically as changes are made in other places (such as settings). 

For each renderer there is also a `BraveWalletProvider` which is used by the renderer process for interaction from `window.ethereum`.

## Debugging Dapps

You can debug Dapps that aren't compatible with our wallet but do work with MetaMask by using a Proxy object. 
It may also be useful to add logging for the method name in `components/brave_wallet/renderer/brave_wallet_js_handler.cc`.

Paste this into the console of the Dapp that isn't working:

```
const handler = {
  get: function(target, prop, receiver) {
    console.log('accessing prop: ', prop, ...arguments)
    if (typeof target[prop] === 'function') {
      return new Proxy(target[prop], {
        apply: (targetFunc, thisArg, args) => {
          console.log('args: ', ...args)
          return targetFunc.call(target, ...args);
        }
      });
    }
    return Reflect.get(...arguments);
  },
  set(target, prop, val) { // to intercept property writing
    console.log('Setting prop to', prop, val, ...arguments)
    return Reflect.set(...arguments);
  },
  apply: function(target, thisArg, args) {
    console.log('apply on target with args: ', target, ...args)
    return target(...args)
  }
};

const ethereumProxy = new Proxy(window.ethereum, handler);
window.ethereum = ethereumProxy
```
Note: This won't work if you have default wallet set to `Brave Wallet`. It needs to be set to `Brave Wallet (Prefer extensions)` because otherwise `window.ethereum` is not writable. 

## HD Wallet

Bitcoin and Ethereum generate addresses in different ways, but they share the same key derivation implementations as BIP-32 compatible wallets. 

The Brave wallet implements BIP-32 (HD wallet), BIP-39 (Mnemonic keywords), BIP-43 (Multipurpose HD wallet structure), and BIP-44 (Multicurrency and multi account wallets).



1. Generate mnemonic words from random 128-256bits (BIP39 https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)
2. Generate seed (512 bits) from the mnemonic words in step 1 with an empty passphrase (salt `mnemonic`) using PBKDF2 (BIP39).  Note: The test vectors use passphrase TREZOR (salt `mnemonicTREZOR`).
3. Use the seed to generate the master key / root key (256bits) and master chain code (256 bits) (BIP32 https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)
4. Use the master private key above to generate a master public key using secp256k1
5. Derive the extended private key and public key at least with 3 hardened derivations and 1 normal derivation from master keys before we can use the keys (BIP44 https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki)
m / purpose' / coin_type' / account' / change / address_index
Where purpose’ is 44'
coin_type’ is 60' (See https://github.com/satoshilabs/slips/blob/master/slip-0044.md#registered-coin-types for example Ethereum is 60’ and Ethereum Classic is 61’ Bitcoin is 0’, Binance Smart Contract is 519’, Binance is 714’). Note that MetaMask uses 60’ for the key path no matter which networks you add. We do the same for compatibility.
account’ is : 0'
change is: 0

Account 0
Private key: m/44'/60'/0'/0/0
Public key: M/44'/60'/0'/0/0

…

Account N
Private key: m/44'/60'/0'/N/0
Public key: M/44'/60'/0'/N/0


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

## JSON RPC API

The following JSON RPC API is exposed:
https://eth.wiki/json-rpc/API#json-rpc-methods

This API will be used by the in page provider (`window.ethereum` object) and also exposed to the Brave Wallet WebUI via mojo.  


## Testing Dapp providers


`window.ethereum` and `window.solana` are only available on secure contexts.
`localhost` and `127.0.0.1` is considered secure even when over HTTP.

But if you want to test with some other host name, you can either use an HTTPS server locally, or use this flag:
brave://flags/#unsafely-treat-insecure-origin-as-secure

For example if you add z.com as 127.0.0.1 in your hosts file and then start it on an HTTP server:

<img width="785" alt="Screen Shot 2022-06-14 at 10 14 05 AM" src="https://user-images.githubusercontent.com/831718/173599734-86289c85-5197-49e7-9687-37adcac04215.png">

## Bump Solana Web3 script process (This requires a security review)
1. Bump `package.json` in brave-core
2. `npm install` to get the bumped version in node_modules
3. Clone https://github.com/brave-experiments/Javascript-Secure-Plugin
4. Copy `node_modules/@solana/web3.js/lib/index.iife.min.js` into the above repo as `solana-web3.js`
5. `npm install` and `npm run build` in the plugin repo
6. Edit the `Build/test.js` to remove lines above `/************************************************************************/` and the last line `/******/ })()` 
7. Add Brave license and `/* Auto generated from @solana/web3.js 1.47.1 index.iife.min.js */` (replace 1.47.1 with the bumped version)
8. Copy `Build/test.js` to brave-core in `components/brave_wallet/resources/solana_web3_script.js`
9. sync and build brave-core
10. Go to https://github.com/darkdh/solana-provider-test to test `connect` and `send transaction` without any console error.
11. If there is no errors, then go ahead and open the PR, otherwise go to step 12.
12. Trace the `solana_web3` error line number from console
13. Looking for something that begins with `$` and add missing builtins into `components/safe_builtins/renderer/safe_builtins.cc`.
  - Let say if it is `$Array.from()` then add it to the the second array of `$Array` block because it is a dynamic method.
  - If it is `Array.prototype.flat()` then add `flat` to the first array of `$Array` block as static method.
15. Repeat step 12-14 until there is no errors and open a PR.


## QA test projects

- https://github.com/bbondy/eth-manual-tests
- https://github.com/darkdh/solana-provider-test
