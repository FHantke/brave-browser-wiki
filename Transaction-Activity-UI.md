# Purpose
- Display an aggregated view of all transactions (Txs) across networks and addresses
- Provide filters to show a subset of wallet history
    - Accounts: filter list to only show activity from the selected account
    - Networks: filter list to show activity from the selected network
- Provide a search-box to filter the listed Txs to items that match the search term


# Which Transactions are displayed?
For a transaction/activity to appear within this page, all of the following MUST be true:
- The Tx must have been initiated from the current Profile's Brave Wallet
- The network filter must be set to either:
    - the same value as the Tx's network (Ethereum, if the Tx occurred on Ethereum main-net)
    - "All Networks"
- The transaction must not have been rejected by the user

# Search-able fields
The following activity data is checked against the provided search-field value for filtering:
- Sent/Received/Swapped asset ticker/symbol (ie. ETH)
- Sent/Received/Swapped asset contract address (ie. 0x.....)
- Sent/Received/Swapped asset name (ie. Ethereum)
- Sender/Receiver address (ie. 0x.....)
- Sender/Receiver address labels (ex. "Primary EVM Account")
- "Intent" label (ie. SEND, RECEIVE, SWAP)
- Transaction hash (ie. 0x.....)
- Transaction approval target address (ie. 0x.....)
- Transaction origin info (ie. uniswap.org)



# Limitations
- [We currently don't show "all" transactions, since we only fetch the selected network for all coin types (not all networks)](https://bravesoftware.slack.com/archives/C023VS4HJ6Q/p1680806694383499?thread_ts=1680806641.725749&cid=C023VS4HJ6Q)
- We can only store the last 10 **confirmed** transactions and last 10 **rejected** transactions per chainId****