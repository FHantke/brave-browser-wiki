Brave Wallet was created as part of the Brave browser in order to bring cryptocurrency and web 3 access to users.
Users can access the Brave Wallet by navigating to brave://wallet or from the wallet icon at the top of the browser.


## Settings

Brave Wallet settings can be accessed from: brave://settings/wallet

#### Default wallet

The default wallet setting can be set to one of the following values:

- `Brave Wallet (Prefer extensions)`: This is the default setting. With this setting, `window.ethereum` is exposed by Brave Wallet; however, extensions such as MetaMask are still allowed to overwrite `window.ethereum`.
- `Brave Wallet`: With this setting `window.ethereum` is exposed by Brave Wallet and extensions do not have access to overwrite `window.ethereum`.
- `Crypto Wallets (Deprecated)`: This setting only shows up for existing users that have used Crypto Wallets in the past.  Crypto Wallets was Brave's old cryptocurrency wallet and it was a MetaMask fork. If this setting is selected, navigating to brave://wallet will load Crypto Wallets instead of Brave Wallet. New users can access this setting only if they have brave://flags/#ethereum_remote-client_new-installs set to true manually.
- `None`: With this setting users can still navigate to brave://wallet, however no `window.ethereum` object is exposed.



