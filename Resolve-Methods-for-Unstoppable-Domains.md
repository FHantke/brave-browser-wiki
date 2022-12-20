This page describes the resolve methods Brave supports for resolving domain names when visiting Unstoppable Domains (ex: .crypto domains).
Users can find the setting by visiting `brave://settings/web3` and look for `Resolve Unstoppable Domains domain names`.

1. `Ask`: This is the default option which Brave would ask whether to enable Unstoppable Domains support when visiting .crypto domains.
2. `Disabled`: Disable the support of Unstoppable Domains.
3. **(Removed in 1.40)** `DNS over HTTPS`: Brave will be using Cloudflare to resolve .crypto domain name lookup requests using DNS over HTTPS. If you enable this, Cloudflare will see the .crypto domain that you're trying to visit but they will not be able to see other domains. See Cloudflare's [terms of use](https://www.cloudflare.com/en-ca/distributed-web-gateway-terms/) and [privacy policy](https://www.cloudflare.com/en-ca/privacypolicy/).
4. `Enabled`: Brave will be using Infura to issue Ethereum JSON-RPC call to the smart contract from Unstoppable Domains to resolve .crypto domain name lookup requests. If you enable this, Infura will see the .crypto domain that you're trying to visit but they will not be able to see other domains. See Infura's [terms of use](https://consensys.net/terms-of-use) and [privacy policy](https://consensys.net/privacy-policy/).

**Note:**
- Since 1.40 in addition to .crypto Brave also supports .x, .nft, .dao, .wallet, .blockchain and .bitcoin domains. 
- Since 1.44 .zil is also supported
