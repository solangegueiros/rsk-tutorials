# Setup the gas price to use with Truffle

**Gas** is the internal pricing for running a transaction or contract. 
When you send tokens, interact with a contract, send R-BTC, or do anything else on the blockchain, you must pay for that computation. 
That payment is calculated as gas. 
In RSK, this is paid in **R-BTC**.
The **minimumGasPrice** is written in the block header by miners and establishes the minimum gas price that a transaction should have in order to be included in that block.

To update the **minimumGasPrice** in your project, run this query in a POSIX compatible terminal, using cURL:

<!-- tabs:start -->
#### ** RSK Testnet **

```shell
curl https://public-node.testnet.rsk.co/ -X POST -H "Content-Type: application/json" \
    --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest",false],"id":1}' \
    > .minimum-gas-price-testnet.json
```

#### ** RSK Mainnet **

```shell
curl https://public-node.rsk.co/ -X POST -H "Content-Type: application/json" \
    --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest",false],"id":1}' \
    > .minimum-gas-price-mainnet.json
```

<!-- tabs:end -->

This query saved the details of the latest block to 
file .minimum-gas-price-testnet.json 
or .minimum-gas-price-mainnet.json, respectively.

In the `truffle-config.js` file, we are reading the parameter `minimumGasPrice` in each json file.

For more information about the **Gas** and **minimumGasPrice** please go to the 
[gas page](https://developers.rsk.co/rsk/rbtc/gas/ "Gas - RSK Developers Portal") 
in the RSK Developers Portal.


> [!ATTENTION]
> TODO
> 
> Show the minimumGasPrice in truffle-config

