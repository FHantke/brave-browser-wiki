Brave supports [ENS(.eth)](https://docs.ens.domains/) domain names resolution. This support includes [resolving wallet addresses](https://docs.ens.domains/ens-improvement-proposals/ensip-1-ens#addr) in wallet panel and [resolving ipfs links](https://docs.ens.domains/ens-improvement-proposals/ensip-7-contenthash-field) in omnibox. ENS is deployed on the Ethereum main network.

Brave also supports transparent reading of [ENS records from offchain storages](https://docs.ens.domains/dapp-developer-guide/ens-l2-offchain). Offchain storage is setup by domain owner by providing a custom resolver smart contract to ENS.

ENS offchain lookup is controlled by settings in brave://settings/extensions. Possible values are:
1. `Ask`. This is the default option which Brave would ask whether to enable ENS offchain lookup when visiting such .eth domains.
2. `Disabled`. Completely disable ENS offchain lookup. Records for such ENS domains will not be resolved by Brave.
3. `Enabled`. Enables ENS offchain lookup. Brave will fetch data from offchain gateways specified by domain owners in their resolver smart contracts.