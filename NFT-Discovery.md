## What is NFT Discovery?
NFT discovery is an optional feature of the Brave Wallet designed to streamline your NFT management process. When enabled, it detects NFTs owned by your wallet addresses and adds them to their wallet. By enabling NFT discovery, you can easily view and manage their NFTs without having to manually add them.

## Enabling and Disabling NFT Discovery
Users will be prompted during onboarding whether they want to enable NFT discovery. Users can always enable/disable NFT discovery in the settings at [brave://settings/wallet](brave://settings/wallet).

## How Does NFT Discovery Work?
After enabling NFT Discovery, the Wallet will periodically send request(s) which include your wallet address(es) to our partner [SimpleHash](https://simplehash.com/). SimpleHash returns a list of NFTs owned by all wallet addresses managed by Brave Wallet across various networks, including a spam score for each. If the NFT is not detected as spam by SimpleHash, it will be automatically added in the background and be immediately visible in your portfolio.

## Understanding Spam Scores
SimpleHash assigns a spam score to each NFT project as a measure of its legitimacy. If the spam score of the NFT is greater than zero, Brave will not add the NFT. Currently, there is no way to view NFTs labeled spam, however this feature may be added in the future.  You can read about spam scores in SimpleHash [blog post](https://blog.simplehash.com/blog/how-simplehash-fights-nft-spam-using-ai-and-crowdsourcing) or in their [documentation](https://docs.simplehash.com/reference/spam-scores).

## What About Fungible Tokens (ERC20s)?
Automatic discovery of ERC20 and SPL tokens in Brave's [registry](https://www.npmjs.com/package/brave-wallet-lists?activeTab=code) use your configured RPC server APIs with a read only smart contract, not SimpleHash, and is always on.