## Introduction

Brave Wallet is a native Brave Browser implementation of an Ethereum (or any EVM compatible chain) remote client. Please note this is different than the old Brave Ethereum wallet based on a [fork](https://github.com/brave/ethereum-remote-client) of [MetaMask](https://github.com/MetaMask/metamask-extension) (wiki can be found [here](https://github.com/brave/brave-browser/wiki/Brave-Ethereum-Remote-Client-Wallet-Seed-Information)). The native implementation requires less processes to run and may even be extended to support other protocols in the future (e.g. Doge, Solana, etc.).

This protocol adopts the [BIP-32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) (HD-Wallet) specification and maintains its own implementation of a Keyring Controller. HD-Wallet uses the [bitcoin-core](https://github.com/bitcoin/bitcoin) implementations of `secp256k1`, `ripemd160`, and `base58`. The remaining cryptographic wallet functionality is provided by [BorringSSL](https://boringssl.googlesource.com/boringssl/) which is used in Chromium in place of OpenSSL (BorringSSL is maintained by Google).

As in the old MetaMask extension, we adopt [BIP-39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) mnemonic keywords based on a high entropy seed of 256bits.  Mnemonic phrases are encrypted at rest using the AEAD suite [AES-256-GCM-SIV](https://datatracker.ietf.org/doc/html/rfc5297) with a randomized 96-bit nonce, and an encryption key derived by a user passphrase fed into a key derivation function. Mnemonics are generated using a Brave maintained [hard fork]([https://github.com/ElementsProject/libwally-core/commit/cd5b8c404352759d603208e08c59003aaeb9a6fa](https://github.com/ElementsProject/libwally-core/commit/cd5b8c404352759d603208e08c59003aaeb9a6fa)) of [libwally-core](https://github.com/ElementsProject/libwally-core).

Protocols
### Mnemonic and HD Wallet Generation
1. Using `crypto::RandBytes` (a BoringSSL source of strong entropy) we derive a 128bit-256bit seed (currently it is configured at 128bits).
2. Create a salt with the string 'mnemonic'
3. Using PBKDF2 + HMAC-SHA512 with 2048 iterations, take the seed from step 2 and the mnemonic from step 1, derive our 512bit seed.
4. Used the 512bit seed to construct the HD-Wallet using BIP32.


### Encryption of Mnemonic
1. User chooses a strong passphrase. Passwords must be at least 7 characters and contain at least one number and special character, though we STRONGLY RECOMMEND much longer passphrases. 
2. Derive a 256-bit salt using `crypto::RandBytes`
3. Derive 256-bit symmetric key using PBKDF2+HMAC-SHA256 with the passphrase + salt as input. We use 100,000 iterations.
4. Derive a 12-byte (96-bit) initialization vector using `crypto::RandBytes`
5. Encrypt the Mnemonic under AES-256-GCM-SIV mode with the mnemonic, key, and IV as input.

### Auto import from MetaMask and Crypto Wallets
1. User provides the password that they used to unlock MM and CW
2.  We will fetch the data stored in `chrome.storage.local` to get salt, iv and encrypted data
3.  Derived encryption key using PBKDF2 + HMAC-SHA256 with password from step1, salt from step 2  and 10000 iterations
4.  Then we decrypt the encrypted data using key from step 3 with mod AES-GCM along with iv from step 2
5.  When we get the decrypted mnemonic, we will do auto import for user

### Auto import from legacy Crypto Wallets (24 words mnemonic)
TBD

