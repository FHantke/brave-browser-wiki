### What is meant by the ‘Transaction Simulation’ service?
It’s an [API](https://brave.com/glossary/api/) service provided  by a third party, called [Blowfish](https://blowfish.xyz/). It’s a [security platform and service](https://docs.blowfish.xyz/docs/introduction) to help detect and warn you of suspicious transactions and potential scam sites for transactions you make in the Brave Wallet. 

### Current Supported Networks

- Arbitrum One
- Arbitrum Sepolia Testnet
- Avalanche Fuji Testnet
- Avalanche Mainnet
- Base Mainnet
- Base Sepolia Testnet
- Blast Mainnet
- Blast Sepolia
- BNB Chain Mainnet
- Degen Mainnet
- Ethereum Mainnet
- Gnosis Mainnet
- Linea Mainnet
- Optimism Mainnet
- Optimism Sepolia Testnet
- Polygon Amoy Testnet
- Polygon Mainnet
- Sepolia Testnet
- Solana Devnet
- Solana Mainnet
- Solana Testnet
- Zora Mainnet

### How does it work?
 - It checks blocklists to see if the domain and/or contract an end-user is interacting with is dangerous or flagged by Blowfish’s risk engine.
- It uses transaction analysis and machine learning to provide crucial information to the user before signing a transaction without ever having access to your on-chain assets or private keys.

### Why is Transaction Simulation Important to Brave Wallet?
- It will help users stay safe whilst transacting with DApps
- Using Blowfish’s risk scoring and domain intelligence, we can provide extra warnings to users before they sign a potentially risky transaction
- Much better UI for the majority of transactions as the ‘Safe Sign’ UI is only used with Brave Swaps and Matcha.xyz

### What transaction data does Blowfish receive and analyze?
Blowfish receives and analyses what website is prompting an end-user to sign a transaction, together with the wallet address and destination address that are part of the Ethereum transaction. This data is necessary to accurately simulate a pending transaction and help safeguard end-users against fraud. 

--- 
The service is provided by Blowfish and is subject to their [Terms of Service](https://extension.blowfish.xyz/terms) and [Privacy Policy](https://extension.blowfish.xyz/privacy) which you should read.