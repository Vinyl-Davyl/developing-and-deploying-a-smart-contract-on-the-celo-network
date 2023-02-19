## 🛠 Developing and Deploying a Smart Contract on Celo

Celo is a blockchain platform that is designed to make financial tools accessible to everyone, regardless of their location or access to traditional financial services. One of the key features of Celo is its support for smart contracts, which are self-executing contracts with the terms of the agreement between buyer and seller being directly written into lines of code. In this article, we will explore the process of developing and deploying a smart contract on Celo, and provide a code sample to illustrate the process.

**Step 1: Set up your development environment**<br>
To develop and deploy a smart contract on Celo, you will need to have a development environment set up. This will typically involve installing the necessary tools and dependencies, such as Node.js and the Celo CLI. You will also need to set up an account on the Celo network and obtain some testnet CELO and cUSD tokens for testing purposes.

Start by cloning your personal repo or follow up and create a sample project like we have below

```js

  git clone <your-repo-link-here>

```

After cloning or creating a repository, then we definitely have all the contracts required to deploy a Dapp.
Let's install all the dependencies of the dapp by using

```js

  npm install

```

**Step 2: Using CELO CLI**<br>
Initially, we must identify a means to engage with the Celo blockchain. In the context of this article, we will utilize the Celo CLI to achieve this objective.

```js

 npm install -g @celo/celocli

```

CeloCLI offers a straightforward approach for interacting with the Celo blockchain and proves to be a valuable asset for all Celo developers. Additional details about CeloCLI can be obtained ([here](https://docs.celo.org/developer#quickstart)).

Once installed, it is essential to confirm that CeloCLI is directed towards the appropriate endpoint. To verify this, we will undertake the following steps:

```js

 celocli config:get

```

![](./assets/config1.jpeg)

Our node should be pointing to node: https://alfajores-forno.celo-testnet.org

In case your endpoint is something else, you can change it by using

```js

  celocli config:set --node https://alfajores-forno.celo-testnet.org

```

At present, our CeloCLI is linked to a Testnet, but in order to proceed, we require an account to work with. Fortunately, we can utilize CeloCLI to create a new account on the testnet by executing the following command:

```js

   celocli account:new

```

Then our terminal should look something like this. (Definetly! Everyone will have different values though!)

![](./assets/config2.jpeg)

To ensure the confidentiality of our private key, developers commonly utilize `.env` files to configure environment variables, which are hidden during the uploading of our project to an open source platform by adding them to the `.gitignore` file. In the same directory, create a `.env` file and enter the following line:

`PRIVATE_KEY=<YOUR_PRIVATE_KEY>`

We will use this private key later in this tutorial. Now that we have an account, we need to obtain funds for signing transactions on the blockchain. To do so, we will use the Alfajores testnet faucet, which can be accessed here. To verify that we have received funds, use the following command:

`celocli account:balance <YOUR_PUBLIC_ADDRESS>`

![](./assets/config3.jpeg)


**Step 3: Write your smart contract**<br>
Once your development environment is set up, you can start writing your smart contract code. The code sample below shows a simple smart contract that allows users to store and retrieve a string value on the Celo blockchain:

```js

const Web3 = require('web3');

const myContractAbi = <ABI>; // Replace with your own ABI
const myContractAddress = '<ContractAddress>'; // Replace with your own contract address
const myCeloAddress = '<YourCeloAddress>'; // Replace with your own Celo address
const myCeloPrivateKey = '<YourCeloPrivateKey>'; // Replace with your own Celo private key

const web3 = new Web3('<CeloNodeUrl>'); // Replace with your own Celo node URL
const myContract = new web3.eth.Contract(myContractAbi, myContractAddress);

async function setString(_myString) {
  const setStringFunction = myContract.methods.setString(_myString);
  const gas = await setStringFunction.estimateGas({ from: myCeloAddress });
  const tx = {
    from: myCeloAddress,
    to: myContractAddress,
    gas,
    data: setStringFunction.encodeABI(),
  };
  const signedTx = await web3.eth.accounts.signTransaction(tx, myCeloPrivateKey);
  const receipt = await web3.eth.sendSignedTransaction(signedTx.rawTransaction);
  return receipt.transactionHash;
}

async function getString() {
  return myContract.methods.getString().call();
}


```

Yeah, Crazy Right? Give it a chill. Now lets explain the code above.

This code is written in JavaScript and uses the Web3.js library to interact with a smart contract on the Celo network. Here is a breakdown of what the code does:

- The first line imports the Web3.js library, which is a JavaScript library used for interacting with the Celo  network.


```js

   const Web3 = require('web3');

```

- The next four lines declare some variables that will be used later in the code. `myContractAbi` is the ABI (Application Binary Interface) of the smart contract, which defines its functions and how to interact with them. `myContractAddress` is the address of the smart contract on the Celo network. `myCeloAddress` is the address of the account that will be used to interact with the smart contract, and `myCeloPrivateKey` is the private key of that account, which is used to sign transactions.

```js

   const myContractAbi = <ABI>; // Replace with your own ABI
   const myContractAddress = '<ContractAddress>'; // Replace with your own contract address
   const myCeloAddress = '<YourCeloAddress>'; // Replace with your own Celo address
   const myCeloPrivateKey = '<YourCeloPrivateKey>'; // Replace with your own Celo private key

```

- The next line creates a new instance of the Web3 object, which is used to interact with the Celo network. `<CeloNodeUrl>` should be replaced with the URL of a Celo node, which is used to communicate with the Celo network.

```js

  const web3 = new Web3('<CeloNodeUrl>'); // Replace with your own Celo node URL

```

- The next line creates a new instance of the smart contract by passing in its ABI and address. This allows you to interact with the smart contract's functions using the `myContract` object.

```js

  const myContract = new web3.eth.Contract(myContractAbi, myContractAddress);

```

- The `setString` function allows you to call the `setString` function on the smart contract. It takes a string parameter, `_myString`, and returns a transaction hash.

```js

  vbnetCopy codeasync function setString(_myString) {
  const setStringFunction = myContract.methods.setString(_myString);
  const gas = await setStringFunction.estimateGas({ from: myCeloAddress });
  const tx = {
    from: myCeloAddress,
    to: myContractAddress,
    gas,
    data: setStringFunction.encodeABI(),
  };
  const signedTx = await web3.eth.accounts.signTransaction(tx, myCeloPrivateKey);
  const receipt = await web3.eth.sendSignedTransaction(signedTx.rawTransaction);
  return receipt.transactionHash;
}


```

The function first creates a new instance of the `setString` function by calling `myContract.methods.setString(_myString)`. Then it estimates the gas required for the transaction and creates a transaction object that includes the `from` address, `to` address, gas amount, and encoded data. It then signs and sends the transaction, and returns the transaction hash.

- The `getString` function allows you to call the `getString` function on the smart contract. It does not take any parameters and returns the value of the `myString` variable stored on the smart contract.

```js

    javascriptCopy codeasync function getString() {
    return myContract.methods.getString().call();
    }

```

The function first creates a new instance of the `getString` function by calling `myContract.methods.getString()`. It then calls the `call()` function to execute the function on the smart contract and return the value of the `myString` variable.

**Step 4: Compile and deploy your smart contract**<br>
Once you have written your smart contract code, you will need to compile it using the Celo CLI. The following command can be used to compile the contract:

```js

    celoc compile MyContract.sol

```

This will generate a bytecode file and an ABI file for your contract. The bytecode is the machine code that will be executed on the Celo blockchain, while the ABI is the interface that other contracts and applications can use to interact with your contract.

Next, you will need to deploy your contract to the Celo network. This can be done using the following command:

```js

    celoc deploy MyContract --args "arg1,arg2"

```

In this command, `arg1` and `arg2` are any constructor arguments that your contract requires. Once the contract is deployed, you will receive a contract address that you can use to interact with your contract.

**Step 5: Interact with your smart contract**<br>
Now that your contract is deployed, you can interact with it using the Celo CLI or a web3 provider. For example, you can use the following command to call the `setString` function and set the value of `myString`:

```js

    celoc contract:call <contract-address> setString --args "Hello, World!"

```

You can then use the following command to call the `getString` function and retrieve the current value of `myString`:

```js

    celoc contract:call <contract-address> getString

```

This will return the string "Hello, World!".

<br><br>
