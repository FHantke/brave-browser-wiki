Brave injects a `windows.ethereum` provider object on all pages.  
This object is defined by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193).

The in page provider will not be provided by Brave in private and Tor window.

This object gives websites the ability to:
- Make requests to an Ethereum node (or a compatible network) to read data from the blockchain
- Request permission to 1 or more Ethereum accounts
- Ask the user (if given permission previously) to sign / submit a transaction
- Ask the user (if given permission previously) to sign a message

# Provider methods

### `window.ethereum.request`

```ts
interface RequestArguments {
  readonly method: string;
  readonly params?: readonly unknown[] | object;
}

Provider.request(args: RequestArguments): Promise<unknown>;
```

### `window.ethereum.isConnected`

```ts
Provider.isConnected(): boolean;
```

# Provider events

### `connect`

The Provider emits connect when it:

- first connects to a chain after being initialized.
- first connects to a chain, after the disconnect event was emitted.

```ts
interface ProviderConnectInfo {
  readonly chainId: string;
}
```

Provider.on('connect', listener: (connectInfo: ProviderConnectInfo) => void): Provider;

### `disconnect`

The Provider emits disconnect when it becomes disconnected from all chains.

```
Provider.on('disconnect', listener: (error: ProviderRpcError) => void): Provider;
```

### `chainChanged`

The Provider emits chainChanged when connecting to a new chain.

```ts
Provider.on('chainChanged', listener: (chainId: string) => void): Provider;
```

### `accountsChanged`

The Provider emits accountsChanged if the accounts returned from the Provider (eth_accounts) change.

```ts
Provider.on('accountsChanged', listener: (accounts: string[]) => void): Provider;
```


# Permissions

Websites can call:

```ts
window.ethereum.request({ method: 'eth_requestAccounts' })
```

To make a request for permissions to an account.
If granted, the website will be able to see the allowed account address.
The website will also be able to ask the user to approve (sing / send) transactions and to sign data.
Signing transactions and messages require separate approval after the initial account approval.

Permissions can be revoked in brave://settings/content/ethereum
A user can also open up the wallet panel and disconnect a connected site when they are on that site. 

If a wallet is not yet setup and a page requests permissions, we will open brave://wallet for the user to setup the wallet.

# Adding other chains

Websites can request that alternate chains be added by using:

```ts
window.ethereum.request({ method: 'wallet_addEthereumChain' }, params)
```


`wallet_addEthereumChain` accepts a single object parameter, specified by the following TypeScript interface:

```ts
interface AddEthereumChainParameter {
  chainId: string;
  blockExplorerUrls?: string[];
  chainName?: string;
  iconUrls?: string[];
  nativeCurrency?: {
    name: string;
    symbol: string;
    decimals: number;
  };
  rpcUrls?: string[];
}
```

# Switching to other chains

Websites can request that the browser changes to a different chain by using `wallet_switchEthereumChain`

```ts
window.ethereum.request({ method: 'wallet_switchEthereumChain' }, params)
```


`wallet_switchEthereumChain` accepts a single object parameter, specified by the following TypeScript interface:

```ts
interface SwitchEthereumChainParameter {
  chainId: string;
}
```

# Sending transactions

Sites can request that a transaction be signed / sent by using the `eth_sendTransaction` method.

For more information, see: https://eth.wiki/json-rpc/API#eth_sendtransaction

# Signing data

Signing data can be done with:

- `eth_sign`
- `personal_sign`
- `signTypedData`
- `signTypedData_v1` (same as `signTypedData`)
- `signTypedData_v3`
- `signTypedData_v4`

These are not implemented yet in Brave and they are being tracked here: https://github.com/brave/brave-browser/issues/17986

# Suggesting an asset 
Websites can suggest an asset to be added into the user wallet via a single `WatchAssetParameters` object. Please see [EIP-747](https://eips.ethereum.org/EIPS/eip-747) for more information.
```ts
interface WatchAssetParameters {
  type: string; // The asset's interface, e.g. 'ERC20'
  options: {
    address: string; // The hexadecimal Ethereum address of the token contract
    symbol?: string; // A ticker symbol or shorthand, up to 5 alphanumerical characters
    decimals?: number; // The number of asset decimals
    image?: string; // A string url of the token logo
  };
}
```
```ts
window.ethereum.request({ method: 'wallet_watchAsset' }, params)
```
Brave will show an UI with the asset to be added for users to accept or cancel the request.
If the same contract address exists in user's current asset list or Brave's build-in list, we will use the information stored in the list instead of from the API request.

Note that image parameter is not supported yet, it is being tracked here: https://github.com/brave/brave-browser/issues/20000 

# Provider properties

### `selectedAddress` (Deprecated)

The string address of the first allowed address or undefined if no account is currently allowed.  Note that when the keyring is locked it is undefined.  Since this property is deprecated, this property is provided for webcompat reasons only.

### `chainId` (Deprecated)

The chain ID of the currently connected network.
Since this property is deprecated, this property is provided for webcompat reasons only.

### `networkVersion` (Deprecated)

A string of the chain ID in base 10.
Since this property is deprecated, this property is provided for webcompat reasons only.

### `_metamask.isUnlocked` (Experimental MM method)

Returns a promise which resolves to true or false depending on if the wallet is locked.
This function is only provided for webcompat reasons only.


# Legacy Provider methods

A number of legacy provider methods are provided for backwards compatibility:

### `enable` (deprecated)

Allows a website to request permissions.

This method is superseded by a `request` with `eth_requestAccounts`.

```ts
Provider.request({ method: 'eth_requestAccounts' })
```

### `sendAsync` (deprecated)

```ts
Provider.sendAsync(request: Object, callback: Function): void;
```

This method is superseded by `request`.

### `send` (deprecated)

```ts
Provider.send(...args: unknown[]): unknown;
```

This method is superseded by `request`.

# Legacy Provider events


### `close` (deprecated)

Not yet implemented, but Brave may implement it.
This event `close` is superseded by `disconnect`.


### `networkChanged` (deprecated)

Not yet implemented, but Brave may implement it.

The event `networkChanged` is superseded by `chainChanged`.

### `notification` (deprecated)

This event is superseded by `message`.

### `message` (deprecated)

Brave has not implemented this event yet. 

The message event is intended for arbitrary notifications not covered by other events.

The event will be emitted with an object argument of the following form:

```ts
interface ProviderMessage {
  readonly type: string;
  readonly data: unknown;
}
```

# Built-in networks

Chain ID | Network name
-------- | ---------------- |
0x1      | Ethereum mainnet |
0x3      | Ropsten Testnet  |
0x3      | Ropsten Testnet  |
0x5      | Rinkeby Testnet  |
0x2a     | Kovan Testnet    |


# Disabling the Ethereum provider object insertion

The Ethereum provider object can be disabled from `brave://settings/wallet` by changing the Default cryptocurrency wallet to `None`.