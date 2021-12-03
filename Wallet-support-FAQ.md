# My MetaMask or other extension is not working with Dapps

Dapps work by communicating with a special object that Brave Wallet and extensions like MetaMask provide named `window.ethereum`.
Only one wallet can provide `window.ethereum` to websites.
In Brave, we expose a setting in brave://settings/wallet to be able to change which wallet provides `window.ethereum`.

Here's a description of each setting:
- Brave Wallet (Prefer extensions) - This is the default. Brave Wallet will expose `window.ethereum` but allow other extensions such as MetaMask to overwrite it.
- Brave Wallet - Exposes `window.ethereum` and prevents sites and extensions from changing `window.ethereum`.
- Crypto Wallets (Deprecated) - Gives access to the old deprecated wallet. This option is not compatible with other extensions such as MetaMask.
- None - `window.ethereum` will not be provided by Brave Wallet at all. If you have extensions such as MetaMask, is is free to use `window.ethereum`.

After changing the default wallet, it is best to restart your browser. Why?
- If you had Crypto Wallets loaded, it won't be unloaded until the next restart. When Crypto Wallets is loaded it will not work properly with other extensions trying to access `window.ethereum`.
- Existing already opened tabs will not change to use the new wallet setting, you need a new tab or a browser restart.

# When I restored my old recovery words, I got a different address than in the old Crypto Wallets

Early versions of the deprecated Crypto Wallets extension used 24-words and were not BIP39 compliant.
This was fixed later on for compatibility with other wallets to be compliant with BIP39.

If you have 24-words and you are restoring your wallet, then a checkbox will appear to use the legacy Brave method.

You should use that checkbox to get back at the same address as your old Crypto Wallets account.

For reference, the difference is:
- 24 words -> entropy and use entropy as seed
- Bip39: Valid mnemonic words->entropy->PBKDF2-HMAC-Sha256 and use that as seed

If you used the wrong method, simply restore on top again. Please remember if you are restoring you will replace and lose what your wallet currently has.  You can restore from the lock screen on the wallet panel or in brave://wallet. 

# Why can't my wallet receive my rewards? How about tipping from my wallet?

BAT Rewards come in the form of virtual BAT (vBAT) until you verify with an exchange. When you verify with an exchange, your vBAT gets deposited there and converted to real BAT.  The step for going to an exchange is required for AML/KYC reasons. We're looking at the decentralization process for Brave ads rewards as part of [Themis](https://brave.com/themis-rfcc-wrap-up/)

Brave Wallet is a self custody wallet and therefore we cannot currently pay to it for the user rev-share for ads. We would need to KYC/AML your address and may also need a money transmitter license to do so.

For tipping, we are planning to implement on-chain P2P tipping. There are some things to work through including high Ethereum gas fees and no anonymity on Ethereum.  After we have Solana support we will likely add this pretty quickly.