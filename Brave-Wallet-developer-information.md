The following information is mostly useful to Brave developers. 
Please see https://github.com/brave/brave-browser/wiki/Brave-Wallet for general information about the Brave Wallet.


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
