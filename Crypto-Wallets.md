# Crypto Wallets

## Install of Crypto Wallets on first use

A user can install Crypto Wallets by either navigating to `brave://wallet`, or by going to a Dapp and confirming to install via the infobar that comes up.

## Dapp detection

Before Crypto Wallets is installed, the browser does something called Dapp detection.
This is the process of injecting a content script to see if window.web3 is accessed without actually injecting web3.

If window.web3 is accessed, then we show an infobar to install Crypto Wallets.  If MetaMask is already installed, we prompt the user to see if they'd like to use Crypto Wallets or MetaMask as their Dapp provider.