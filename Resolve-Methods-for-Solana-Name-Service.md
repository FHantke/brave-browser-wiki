This page describes the resolve methods Brave supports for resolving domain names when visiting Solana name service domains (ex: .sol domains).
Users can find the setting by visiting `brave://settings/web3` and look for `Resolve Solana Name Service (SNS) domain names`.

1. `Ask`: This is the default option which Brave would ask whether to enable Solana name service support when visiting .sol domains.
2. `Disabled`: Disable the support of Solana name service.
3. `Enabled`: Brave will be using a third-party to issue Solana JSON-RPC calls in order to resolve .sol domain name lookup requests. Brave will hide your IP address from the third-party. If you enable this, the third-party will see that someone is trying to visit a .sol domain you go to but they will not be able to see other domains. Please see below for the third-parties Brave currently using.

Third-party providers (please note that this list could be changed overtime):
1. Syndica: [terms of use](https://syndica.io/terms-and-conditions/) and [privacy policy](https://syndica.io/privacy-policy/).
2. Chainstack: [terms of use](https://chainstack.com/tos/) and [privacy policy](https://chainstack.com/privacy/).