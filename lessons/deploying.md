- ## Deploying your Smart Contract on Celo

In order to deploy our dapp to the Celo blockchain, we simply need to modify the `truffle-config.js` file to be compatible with the Celo blockchain. To accomplish this, we will utilize the Contract Kit and Wallet-local tools provided by Celo. Additionally, we must install `dotenv` to enable the use of our Private Key.Changing Truffle-config

To install all of these, use

```js

    npm i --save @celo/contractkit @celo/wallet-local dotenv

```

`ContractKit` is a library designed to assist developers and validators in interacting with the Celo blockchain, making it an ideal tool for developers seeking a straightforward means of integrating Celo Smart Contracts into their applications. Wallet-Local, on the other hand, provides a local wallet instance for use with ContractKit. To import the Private Key we added to the `.env` file into `truffle-config` without hardcoding it, we will use the `dotenv` file.

Insert the code below into our `truffle-config.js` file.

```js

    require("dotenv").config();  // to import all the environment-specific variables into this file
    const ContractKit = require("@celo/contractkit");
    const { LocalWallet } = require("@celo/wallet-local");

    const PRIVATE_KEY = process.env.PRIVATE_KEY;
    const testnetURL = "https://alfajores-forno.celo-testnet.org";
    const localWallet = new LocalWallet();
    /*
    Contractkit is used to access web3 object to interact with node's Json RPC API.
    It takes two arguments, first being testnet url and second a wallet instance for signing transactions.
    */

    const kit = ContractKit.newKit(testnetURL, localWallet);

    async function setConfig() {
    kit.addAccount(PRIVATE_KEY);
    kit.defaultAccount = localWallet.getAccounts()[0];
    }
    setConfig();
    module.exports = {
    networks: {
        testnet: {
        provider: kit.connection.web3.currentProvider, // to connect with Alfajores testnet
        network_id: 44787,
        },
    },
    };


```

Now our configuration file is all set to work with the Celo Blockchain.

Let's compile our smart contracts using

```js

    Truffle compile

```

and after compiling is completed, let's migrate our smart contracts to Celo.

```js

    Truffle migrate --network testnet

```

Congratulations on successfully deploying our Ethereum Dapp to Celo! 🎊 While it may have been a lengthy article, we made it through the first half.

<br><br>
