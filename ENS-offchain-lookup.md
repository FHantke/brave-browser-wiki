Brave supports transparent reading of [ENS records from offchain storages](https://docs.ens.domains/dapp-developer-guide/ens-l2-offchain) for [resolving wallet addresses](https://docs.ens.domains/ens-improvement-proposals/ensip-1-ens#addr) in the Brave Wallet panel and [resolving IPFS links](https://docs.ens.domains/ens-improvement-proposals/ensip-7-contenthash-field) in the omnibox. Offchain storage is set up by the domain owner by providing a custom resolver smart contract to ENS, which could be a third party. If you enable offchain lookup, a third party could see the .eth domain you're trying to visit but they won't be able to see other domains you visit.

**Allow ENS offchain lookup** preference is controlled in Settings in the **Web3** section, under **Web3 Domains**. Possible values are:
1. `Ask`. This is the default option which Brave would ask whether to enable ENS offchain lookup when visiting such .eth domains.
2. `Disabled`. Completely disable ENS offchain lookup. Records for such ENS domains will not be resolved by Brave.
3. `Enabled`. Enables ENS offchain lookup. Brave will fetch data from offchain gateways specified by domain owners in their resolver smart contracts.

Note that this option will only appear if **Resolve Ethereum Name Service (ENS) domain names** is set to be **Enabled**.