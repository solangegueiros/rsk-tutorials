


# Create a wallet mnemonic

To use testnet, we need tR-BTC and an address to store them.
The best way is to create a wallet from a mnemonic, using the pattern defined at [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)

There are a few ways to do this.

One is to create using a web wallet, 
such as [Metamask](https://metamask.io/) 
or [Nifty](https://www.poa.network/for-users/nifty-wallet) wallet.
These wallets generate the mnemonic for you.



Select the RSK Network in the web wallet.
- Nifty: select in the dropdown list
- Metamask: go to [RSK Testnet](/wallet/use/metamask/) to configure it in `Custom RPC`




# HD wallet provider

To connect to the RSK network, we are going to use a provider that allows us to connect to any network by unlocking an account locally. 
We are using `@truffle/hdwallet-provider`. It will be installed with the Truffle box.

Please be aware that we are using `HDWalletProvider` with RSK Networks derivations path:
- RSK Mainnet dpath: `m/44’/137’/0’/0`
- RSK Testnet dpath: `m/44’/37310’/0’/0`

For more information, check [RSKIP57](https://github.com/rsksmart/RSKIPs/blob/master/IPs/RSKIP57.md).

# Get R-BTC

The Smart Bitcoin (R-BTC) is the token used to pay for the execution of transactions in RSK.

**Mainnet**

For the RSK Mainnet, get R-BTC from [an exchange](https://www.rsk.co/#exchanges-rsk).

**Testnet**

For the RSK Testnet, get tR-BTC from this [faucet](https://faucet.testnet.rsk.co/).
