## Introduction

Brave Wallet is a native Brave Browser implementation of an Ethereum (or any EVM compatible chain) and [Filecoin](https://spec.filecoin.io/) remote client. Please note this is different than the old Brave Ethereum wallet based on a [fork](https://github.com/brave/ethereum-remote-client) of [MetaMask](https://github.com/MetaMask/metamask-extension) (wiki can be found [here](https://github.com/brave/brave-browser/wiki/Brave-Ethereum-Remote-Client-Wallet-Seed-Information)). The native implementation requires less processes to run and may even be extended to support other protocols in the future (e.g. Doge, Solana, etc.).

These protocols adopt the [BIP-32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) (HD-Wallet) specification and maintains its own implementation of a Keyring Controller. HD-Wallet uses the [bitcoin-core](https://github.com/bitcoin/bitcoin) implementations of `secp256k1`, `ripemd160`, `base58`, and `BLS12-381`. The remaining cryptographic wallet functionality is provided by [BorringSSL](https://boringssl.googlesource.com/boringssl/) which is used in Chromium in place of OpenSSL (BorringSSL is maintained by Google).

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
1. User provides the password that they used to unlock CW which has 24 words mnemonic
2. We will fetch the data stored in `chrome.storage.local` to get salt, iv, encrypted data and also argon2 params and salt for argon2
3. Derived new password using argon2 with password from step1 along with argon2 params and argon2 salt
4. Derived encryption key using PBKDF2 + HMAC-SHA256 with password from step3, salt from step 2  and 10000 iterations
5. Then we decrypt the encrypted data using key from step 3 with mod AES-GCM along with iv from step 2
6. When we get the decrypted mnemonic, we will do auto import for user

### Filecoin Addresses

There are 2 ways a filecoin address can be represented. An address appearing on chain will always be formatted as raw bytes. An address may also be encoded to a string, this encoding includes a checksum and network prefix. [More](https://spec.filecoin.io/appendix/address/#section-appendix.address.string)
String:
 |------------|----------|---------|----------|
 |  network   | protocol | payload | checksum |
 |------------|----------|---------|----------|
 | 'f' or 't' |  1 byte  | n bytes | 4 bytes  |
Network:
- 't' means test/calibration or localhost node network.
- 'f' means Mainnet
Procotol:
- value 1: addresses represent secp256k1 public encryption keys. The payload field contains the Blake2b 160 hash of the uncompressed public key (65 bytes).
- value 3: addresses represent BLS public encryption keys.
Payload:
- The payload field contains 48 byte BLS PubKey public key. All payloads except the payload of the ID protocol are base32 encoded using the lowercase alphabet when seralized to their human readable format.
Checksum:
- Filecoin checksums are calculated over the address protocol and payload using blake2b-4. Checksums are base32 encoded and only added to an address when encoding to a string. Addresses following the ID Protocol do not have a checksum.


### Import Filecoin from Hardware Wallets
1. Filecoin adopt the [BIP-32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) (HD-Wallet) specification and maintains its own implementation of a Keyring Controller. It uses the [bitcoin-core](https://github.com/bitcoin/bitcoin) implementations of `secp256k1`, and [bls-signatures](https://github.com/filecoin-project/bls-signatures) for `BLS12-381`. 
2. Derivation paths adopt [SLIP-0044](https://github.com/satoshilabs/slips/blob/5f85bc4854adc84ca2dc5a3ab7f4b9e74cb9c8ab/slip-0044.md). Importing accounts for Mainnet uses `m/44'/461'/0'/0/${i}` derivation paths, and, accordingly, a `m/44'/1'/0'/0/${i}` is used for test networks