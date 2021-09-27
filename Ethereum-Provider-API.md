Brave injects a `windows.ethereum` provider object on all pages.  
This object is defined by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193).

This object gives websites the ability to:
- Make requests to an Ethereum node (or a compatible network) to read data from the blockchain
- Request permission to 1 or more Ethereum accounts
- Ask the user (if given permission previously) to sign / submit a transaction
- Ask the user (if given permission previously) to sign a message

# Provider methods

### `window.ethereum.request`

```js
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

# Legacy Provider methods and events

A number of legacy provider methods are provided for backwards compatibility:

### `enable`

Allows a website to request permissions.

This method is superseded by a `request` with `eth_requestAccounts`.

```js
Provider.request({ method: 'eth_requestAccounts' })
```

### `sendAsync`

```js
Provider.sendAsync(request: Object, callback: Function): void;
```

This method is superseded by `request`.

### `send`

```js
Provider.send(...args: unknown[]): unknown;
```

This method is superseded by `request`.

### event: `close`

Not yet implemented, but Brave may implement it.
This event `close` is superseded by `disconnect`.


### event: `networkChanged` 

Not yet implemented, but Brave may implement it.

The event `networkChanged` is superseded by `chainChanged`.

### event: `notification`

This event is superseded by `message`.

### event: `message`

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