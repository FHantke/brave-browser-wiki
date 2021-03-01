This is how to detect the new Brave Wallet. It is not released yet but is expected to be released sometime in 2021.

```
if (!ethereum || !ethereum.isBraveWallet) {
  throw new Error('Brave Wallet is not available')
}

let currentChainId = null
ethereum.send('eth_chainId')
  .then(handleChainIdChanged)
  .catch(err => console.error(err))

ethereum.on('chainIdChanged', handleChainIdChanged)

function handleChainIdChanged(chainId) {
  if (currentChainId !== chainId) {
    currentChainId = chainId
  }
}

let currentAccount = null
ethereum.send('eth_accounts')
  .then(handleAccountsChanged)
  .catch(err => {
    if (err.code === 4100) {
      console.log('Please connect to Brave first.')
    } else {
      console.error(err)
    }
  })

ethereum.on('accountsChanged', handleAccountsChanged)

function handleAccountsChanged (accounts) {
  if (accounts.length === 0) {
    console.log('Please connect to Brave Wallet.')
  } else if (accounts[0] !== currentAccount) {
    currentAccount = accounts[0]
  }
}

function connect () {
  // This is equivalent to ethereum.enable()
  ethereum.send('eth_requestAccounts')
    .then(handleAccountsChanged)
    .catch(err => {
      if (err.code === 4001) {
        console.log('Please connect to Brave Wallet.')
      } else {
        console.error(err)
      }
    })
}

const button = document.createElement('button')
button.type = 'button'
button.value = 'Connect!'
button.addEventListener('click', connect)
document.body.appendChild(button)
```