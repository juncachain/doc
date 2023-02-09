# Using Hardhat for Deploying Smart Contracts on JuncaChain

In this tutorial, we explain step-by-step how to create, compile and deploy a simple smart contract on the JuncaChain Testnet using Hardhat.

## What is Hardhat

Hardhat is a development environment to compile, deploy, test, and debug your smart contract.

## Setting up the development environment

There are a few technical requirements before we start.

### Pre-requisites

There are a few technical requirements before we start as listed below:

- [Node.js v10+ LTS and npm](https://nodejs.org/en/) (comes with Node)
- [Git](https://git-scm.com/)
- Create an empty project ```npm init --yes```
- Once your project is ready, run ```npm install --save-dev hardhat``` to install Hardhat.
- Install hardhat toolbox ```npm install @nomicfoundation/hardhat-toolbox```
- To use your local installation of Hardhat, you need to use `npx` to run it (i.e. `npx hardhat`).

## Create A Project

- To create your Hardhat project run ```npx hardhat``` in your project folder to intialize your project.
- Select ```Create an empty hardhat.config.js``` with your keyboard and hit enter.

```
$ npx hardhat
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

Welcome to Hardhat v2.10.1

√ What do you want to do? · Create a JavaScript project
√ Hardhat project root: ·  Project-Directory
√ Do you want to add a .gitignore? (Y/n) · y

You need to install these dependencies to run the sample project:
npm WARN config global `--global`, `--local` are deprecated. Use `--location=global` instead.
  npm install --save-dev "hardhat@^2.10.1" "@nomicfoundation/hardhat-toolbox@^1.0.1"

Project created

See the README.md file for some example tasks you can run

Give Hardhat a star on Github if you're enjoying it!

     https://github.com/NomicFoundation/hardhat
```

When Hardhat is run, it searches for the closest ```hardhat.config.js``` file starting from the current working directory. This file normally lives in the root of your project and an empty ```hardhat.config.js``` is enough for Hardhat to work. The entirety of your setup is contained in this file.


## Create Smart Contract

You can write your own smart contract, place it in the ```contracts``` directory of your project and remane it as ```JRC20Token.sol```.

## Configure Hardhat for JuncaChain

- Go to ```hardhat.config.js```
- Update the  config with junca-network-crendentials.

```js
require("@nomicfoundation/hardhat-toolbox");

const { mnemonic } = require('./secrets.json');

// This is a sample Hardhat task. To learn how to create your own go to
// https://hardhat.org/guides/create-task.html
task("accounts", "Prints the list of accounts", async () => {
    const accounts = await ethers.getSigners();

    for (const account of accounts) {
        console.log(account.address);
    }
});

// You need to export an object to set up your config
// Go to https://hardhat.org/config/ to learn more

/**
 * @type import('hardhat/config').HardhatUserConfig
 */
module.exports = {
    defaultNetwork: "mainnet",
    networks: {
        localhost: {
            url: "http://127.0.0.1:8545"
        },
        hardhat: {
        },
        testnet: {
            url: "https://rpc-testnet.juncachain.com/",
            chainId: 667,
            gasPrice: 20000000000,
            accounts: {mnemonic: mnemonic}
        },
        mainnet: {
            url: "https://rpc.juncachain.com/",
            chainId: 668,
            gasPrice: 20000000000,
            accounts: {mnemonic: mnemonic}
        }
    },
    solidity: {
        version: "0.8.9",
        settings: {
            optimizer: {
                enabled: true
            }
        }
    },
    paths: {
        sources: "./contracts",
        tests: "./test",
        cache: "./cache",
        artifacts: "./artifacts"
    },
    mocha: {
        timeout: 20000
    }
};

```

:::note
It requires mnemonic to be passed in for Provider, this is the seed phrase for the account you'd like to deploy from. Create a new `secrets.json` file in root directory and enter your 12 word mnemonic seed phrase to get started. To get the seedwords from metamask wallet you can go to Metamask Settings, then from the menu choose Security and Privacy where you will see a button that says reveal seed words.
```
Sample secrets.json

{
    "mnemonic": "Your_12_Word_MetaMask_Seed_Phrase"
}
```
:::

## Compile Smart Contract

To compile a Hardhat project, change to the root of the directory where the project is located and then type the following into a terminal:

```
npx hardhat compile
```

## Deploy Smart Contract on JuncaChain Network

- Copy and paste the following content into the ```scripts/deploy.js``` file.

```js
async function main() {
    const [deployer] = await ethers.getSigners();

    console.log("Deploying contracts with the account:", deployer.address);

    console.log("Account balance:", (await deployer.getBalance()).toString());

    const Token = await ethers.getContractFactory("JRC20Token"); //Replace with name of your smart contract
    const token = await Token.deploy();

    console.log("Token address:", token.address);
}

main()
    .then(() => process.exit(0))
    .catch((error) => {
        console.error(error);
        process.exit(1);
    });

```

- Run this command in root of the project directory:

```js
$  npx hardhat run --network testnet scripts/deploy.js
```
- Sample Output

```
$ npx hardhat run --network testnet scripts/deploy.js
Deploying contracts with the account: 0x27cf2CEAcdedce834f1673005Ed1C60efA63c081
Account balance: 100721709119999208161
Token address: 0xbF39886B4F91F5170934191b0d96Dd277147FBB2
```
> Remember your address, transaction_hash and other details provided would differ, Above is just to provide an idea of structure.

**Congratulations!** You have successfully deployed JRC20 Smart Contract. Now you can interact with the Smart Contract.

You can check the deployment status here: <https://scan.juncachain.com> or <https://scan-testnet.juncachain.com>