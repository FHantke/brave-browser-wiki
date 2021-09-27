Brave injects a `windows.ethereum` provider object on all pages.

This object gives websites the ability to:
- Make requests to an Ethereum node to read data
- Request permission to 1 or more Ethereum accounts
- Ask the user (if given permission previously) to sign / submit a transaction
- Ask the user (if given permission previously) to sign a message



# Disabling the Ethereum provider object insertion

The Ethereum provider object can be disabled from brave://settings/wallet by changing the Default cryptocurrency wallet to `None`.