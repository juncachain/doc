# How to Run A Fullnode on Juncachain

## Fullnodes Functions

* Stores the full blockchain history on disk and can answer the data request from the network.
* Receives and validates the new blocks and transactions.
* Verifies the states of every account.

## Supported Platforms

We support running a full node on **Linux**.

## Hardware Recommendation
- Directly facing internet
- 16 cores CPU
- 32GB of RAM
- SSD storage

{% hint style="note" %}
If you are running a node in Testnet, 2CPU/8GB of RAM is sufficient.
{% endhint %}

## Steps to Run a Fullnode

### Prepare junca client software <a href="#prepare-junca-client-software" id="prepare-junca-client-software"></a>

**Build from source code¶**

Create new directory for the project

```
mkdir -p $GOPATH/src/github.com/juncachain/
cd $GOPATH/src/github.com/juncachain/
```

* Download source code and build

```
git clone https://github.com/juncachain/juncachain.git juncachain
cd juncachain
```

* Checkout the latest version (e.g v0.2.0)

```
git pull origin --tags
git checkout v0.2.0
```

* Build the project

```
make all
```

* Binary file should be generated in build folder `$GOPATH/src/github.com/juncachain/juncachain/build/bin`

```
alias junca=$GOPATH/src/github.com/juncachain/juncachain/build/bin/junca
```

**Download JuncaChain binary from Github release page**

Download junca binary from our [releases page](https://github.com/juncachain/juncachain/releases) (e.g v0.2.0)

```
tar xf junca-linux-amd64-v0.2.0.tar.gz
alias junca=build/bin/junca
```

### Download genesis block <a href="#download-genesis-block" id="download-genesis-block"></a>

$GENESIS\_PATH : location of genesis file you would like to put

```
export GENESIS_PATH=path/to/genesis.json
```

* Testnet

```
curl -L https://raw.githubusercontent.com/juncachain/juncachain/master/genesis/testnet.json -o $GENESIS_PATH
```

* Mainnet

```
curl -L https://raw.githubusercontent.com/juncachain/juncachain/master/genesis/mainnet.json -o $GENESIS_PATH
```

### Create datadir <a href="#create-datadir" id="create-datadir"></a>

* create a folder to store juncachain data on your machine

```
export DATA_DIR=/path/to/your/data/folder
mkdir -p $DATA_DIR/junca
```

### Initialize the chain from genesis <a href="#initialize-the-chain-from-genesis" id="initialize-the-chain-from-genesis"></a>

```
junca init $GENESIS_PATH --datadir $DATA_DIR
```

### Initialize / Import accounts for the nodes's keystore <a href="#initialize-import-accounts-for-the-nodess-keystore" id="initialize-import-accounts-for-the-nodess-keystore"></a>

If you already had an existing account, import it. Otherwise, please initialize new accounts&#x20;

```
export KEYSTORE_DIR=path/to/keystore
```

**Initialize new accounts**

```
junca account new \
    --password [YOUR_PASSWORD_FILE_TO_LOCK_YOUR_ACCOUNT] \
    --keystore $KEYSTORE_DIR
```

**Import accounts**

```
junca account import [PRIVATE_KEY_FILE_OF_YOUR_ACCOUNT] \    
    --keystore $KEYSTORE_DIR \
    --password [YOUR_PASSWORD_FILE_TO_LOCK_YOUR_ACCOUNT]
```

**List all available accounts in keystore folder**

```
junca account list --datadir $DATA_DIR  --keystore $KEYSTORE_DIR
```

### Start a node <a href="#start-a-node" id="start-a-node"></a>

**Environment variables**

* $IDENTITY: the name of your node
* $PASSWORD: the password file to unlock your account
* $YOUR\_COINBASE\_ADDRESS: address of your account which generated in the previous step
* $NETWORK\_ID: the networkId. Mainnet: 668. Testnet: 667
* $BOOTNODES: The comma separated list of bootnodes. Find them Mainnet [here](https://doc.juncachain.com/developer-guide/mainnet#bootnodes) Testnet [here](https://doc.juncachain.com/developer-guide/testnet#bootnodes)

**Let's start a node**

```
junca  --syncmode "full" \
    --datadir $DATA_DIR --networkid $NETWORK_ID --port 30303 \
    --keystore $KEYSTORE_DIR --password $PASSWORD \
    --identity $IDENTITY \
    --mine --miner.gasprice 250000000 --miner.etherbase $YOUR_COINBASE_ADDRESS \
    --bootnodes $BOOTNODES
```

If you are a dapp developer, you should open RPC and WS apis:

```
junca  --syncmode "full" \
    --datadir $DATA_DIR --networkid $NETWORK_ID --port 30303 \
    --keystore $KEYSTORE_DIR --password $PASSWORD \
    --identity $IDENTITY \
    --mine --miner.gasprice 250000000 --miner.etherbase $YOUR_COINBASE_ADDRESS \
    --http --http.port=8545 \
    --ws --ws.port=8546 \
    --bootnodes $BOOTNODES
```
