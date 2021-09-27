Brave injects a `windows.ethereum` provider object on all pages.  
This object is defined by [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193).

This object gives websites the ability to:
- Make requests to an Ethereum node (or a compatible network) to read data from the blockchain
- Request permission to 1 or more Ethereum accounts
- Ask the user (if given permission previously) to sign / submit a transaction
- Ask the user (if given permission previously) to sign a message

# Provider methods

## `window.ethereum.request`

```js
interface RequestArguments {
  readonly method: string;
  readonly params?: readonly unknown[] | object;
}

Provider.request(args: RequestArguments): Promise<unknown>;
```

## `window.ethereum.isConnected`

```js
Provider.isConnected(): boolean;
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

The Ethereum provider object can be disabled from brave://settings/wallet by changing the Default cryptocurrency wallet to `None`.