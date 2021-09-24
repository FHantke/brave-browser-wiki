## Introduction

Brave Wallet is a native Brave Browser implementation of an Ethereum (or any EVM compatible chain) remote client. Please note this is different than the old Brave Ethereum wallet based on [a fork](https://github.com/brave/ethereum-remote-client) of [MetaMask](https://github.com/MetaMask/metamask-extension) (wiki can be found [here](https://github.com/brave/brave-browser/wiki/Brave-Ethereum-Remote-Client-Wallet-Seed-Information)). The native implementation requires less processes to run and may even be extended to support other protocols in the future (e.g. Doge, Solana, etc.).

This protocol adopts the [BIP-32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) (HD-Wallet) specification and maintains its own implementation of a Keyring Controller. HD-Wallet uses the [bitcoin-core](https://github.com/bitcoin/bitcoin) implementations of `secp256k1`, `ripemd160`, and `base58`. The remaining cryptographic wallet functionality is provided by [BorringSSL](https://boringssl.googlesource.com/boringssl/) which is used in Chromium in place of OpenSSL (BorringSSL is maintained by Google).

As in the old MetaMask extension, we adopt [BIP-39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) mnemonic keywords based on a high entropy seed of 256bits.  Mnemonic phrases are encrypted at rest using the AEAD suite [AES-256-GCM-SIV](https://datatracker.ietf.org/doc/html/rfc5297) with a randomized 96-bit nonce, and an encryption key derived by a user passphrase fed into a key derivation function. Mnemonics are generated using a Brave maintained [hard fork]([https://github.com/ElementsProject/libwally-core/commit/cd5b8c404352759d603208e08c59003aaeb9a6fa](https://github.com/ElementsProject/libwally-core/commit/cd5b8c404352759d603208e08c59003aaeb9a6fa)) of [libwally-core](https://github.com/ElementsProject/libwally-core).

## Protocols

### Mnemonic generation and encryption

1. User chooses a strong passphrase


### HD-Wallets
1. 



