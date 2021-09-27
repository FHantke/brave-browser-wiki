Brave injects a `windows.ethereum` provider object on all pages.  
This object is defined by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193).

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

```js
Provider.isConnected(): boolean;
```

# Provider events

### `connect`

The Provider emits connect when it:

- first connects to a chain after being initialized.
- first connects to a chain, after the disconnect event was emitted.

```js
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

```js
Provider.on('chainChanged', listener: (chainId: string) => void): Provider;
```

### `accountsChanged`

The Provider emits accountsChanged if the accounts returned from the Provider (eth_accounts) change.

```js
Provider.on('accountsChanged', listener: (accounts: string[]) => void): Provider;
```


# Permissions

Websites can call:

```js
window.ethereum.request({ method: 'eth_requestAccounts' })
```

To make a request for permissions to an account.
If granted, the website will be able to see the allowed account address.
The website will also be able to ask the user to approve (sing / send) transactions and to sign messages.
Signing transactions and messages require separate approval after the initial account approval.

# Adding other chains

Websites can request that alternate chains be added by using:

```js
window.ethereum.request({ method: 'wallet_addEthereumChain' }, params)
```


`wallet_addEthereumChain` accepts a single object parameter, specified by the following TypeScript interface:

```js
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

# Legacy Provider methods

A number of legacy provider methods are provided for backwards compatibility:

### `enable` (deprecated)

Allows a website to request permissions.

This method is superseded by a `request` with `eth_requestAccounts`.

```js
Provider.request({ method: 'eth_requestAccounts' })
```

### `sendAsync` (deprecated)

```js
Provider.sendAsync(request: Object, callback: Function): void;
```

This method is superseded by `request`.

### `send` (deprecated)

```js
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

```js
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