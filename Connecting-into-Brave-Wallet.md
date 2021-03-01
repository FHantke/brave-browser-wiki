This is how to detect Brave Wallet from a web page, to connect, and to get the connected accounts.

```
// Only the new Brave native wallet in 2021 will have ethereum.isBraveWallet
// It will have both isBraveWallet true and isMetaMask true since it is compatible 
// with MetaMask.  It will do this for better webcompat.
if (!ethereum || (!ethereum.isBraveWallet && !ethereum.isMetaMask)) {
  throw new Error('Brave Wallet is not available')
}

let currentChainId = null
function getCurrentChainId() {
  ethereum.request({ method: 'eth_chainId' })
    .then(handleChainChanged)
    .catch(err => console.error(err))
}

ethereum.on('chainChanged', handleChainChanged)

function handleChainChanged(chainId) {
  if (currentChainId !== chainId) {
    currentChainId = chainId
  }
}

let currentAccount = null
function getAccounts() {
  ethereum.request({ method: 'eth_accounts' })
    .then(handleAccountsChanged)
    .catch(err => {
      if (err.code === 4100) {
        console.log('Please connect to Brave first.')
      } else {
        console.error(err)
      }
    })
}

ethereum.on('accountsChanged', handleAccountsChanged)

function handleAccountsChanged (accounts) {
  console.log('handleAccounts: ', accounts)
  if (accounts.length === 0) {
    console.log('Please connect to Brave Wallet.')
  } else if (accounts[0] !== currentAccount) {
    currentAccount = accounts[0]
    console.log('current account is: ', currentAccount)
  }
}

function connect () {
  // This is equivalent to ethereum.enable()
  ethereum.request({ method: 'eth_requestAccounts' })
    .then(handleAccountsChanged)
    .catch(err => {
      if (err.code === 4001) {
        console.log('Please connect to Brave Wallet.')
      } else {
        console.error(err)
      }
    })
}

const connectButton = document.createElement('button')
connectButton.innerHTML = ' Connect! '
connectButton.addEventListener('click', connect)
document.body.appendChild(connectButton)

const accountsButton = document.createElement('button')
accountsButton.innerHTML = ' Get accounts to console! '
accountsButton.addEventListener('click', getAccounts)
document.body.appendChild(accountsButton)

getCurrentChainId()

```