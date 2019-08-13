# High level overview

Brave implements [a fork](https://github.com/brave/ethereum-remote-client) of [MetaMask](https://github.com/MetaMask/metamask-extension) named Ethereum Remote Client to provide wallet functionality and Dapp browsing.

One major difference of this fork, is the way the seed key is generated. In particular it uses 24-word BIP39 format. The way the seed is generated differs and is handled by Brave Core.

We maintain our own fork of [KeyringController](https://github.com/brave/KeyringController) which uses a different [eth-hd-keyring](https://github.com/brave/eth-hd-keyring) dependency.

`eth-hd-keyring` makes a call to `chrome.getWalletSeed(key, callback)` when it would like to generate a new seed.
This API is exposed only to our fork of MetaMask and is implemented in Brave Core.  Where `callback` has a single parameter with an array buffer holding the seed to use in the wallet. `key` is derived from the passphrase in the extension during setup.

Note that Metamask uses https://github.com/danfinlay/browser-passworder/blob/master/index.js#L22 which uses 10000 iterations of pbkdf2 sha256 to derive the key from the user-supplied input. This is used in metamask with AES-GCM to encrypt the serialized keyring when it is stored on disk (and is decrypted by metamask on startup after the user enters their password).

`chrome.getWalletSeed` does the following in C++:

- Generates a master seed (a 32-byte random array buffer using `crypto/random.h` and use `crypto::RandBytes`.
- Uses the passphrase-derived `key` to encrypt the master seed using `AES-GCM-SIV-256` with a random 12-byte nonce.
- Stores the encrypted master seed and nonce on preferences: `brave.wallet.aes_256_gcm_siv_nonce`, `brave.wallet.encrypted_seed`.
- Calls the callback function supplied as the second parameter with an array buffer. The array buffer has 32 bytes of output from HKDF-SHA256(masterseed, info='ethwallet') to the extension, asynchronously.
- Future calls to getWalletSeed (after a seed has already been generated) will decrypt the seed stored on disk using the passphrase and return the output above.


## Protocol

All key/seed/hash lengths are 32 bytes. HKDF salt is empty unless otherwise specified. HKDF uses sha-512 unless otherwise specified.

1. user picks a passphrase
2. passphrase is argon2id hashed to derive a master key M. current parameters: salt length = 32 bytes, time = 1, memory = 0.5G. This takes about 1.4s per hash on a top-tier 2018 macbook pro.
3. M is HKDF'ed into two subkeys, one for metamask and one for brave-core (more on the difference in a sec). Call these subkeys `S_m = HKDF(M, info=’metamask-encryptor’)` and `S_b=HKDF(M, info=’ethwallet-encryptor’)`
4. S_m is used in place of what metamask upstream uses the raw passphrase for. so this gets put through their key derivation function (PBKDF2) to derive an AES-GCM encryption key. This AES-GCM key is used to encrypt the metamask wallet private key on disk after it is generated later. I realize the PBKDF2 step is totally unnecessary so we could remove it if we wanted to, but that's slightly more work than just keeping it.
5. S_b is passed to brave-core and used as an AES-GCM-SIV key to encrypt the "master seed" on disk. Nonce is unnecessary here but generated randomly (12 bytes) and stored on disk anyway.
6. The "master seed" is 32 random bytes generated in C++. This seed is intended to be used to derive other secret key material in the future (sync, rewards, etc.) as well as for metamask. Right now it's only used for metamask.
7. In C++, the "master seed" is HKDF'ed to produce a private key which is passed back to metamask to be used as the wallet private key. Specifically, using `HKDF-SHA256(masterseed, info='ethwallet', salt='brave-ethwallet-salt')` 
