# JSON-RPC Endpoint

{% hint style="info" %}
The rate limit of Junca endpoint on Testnet and Mainnet is 10K/5min.
{% endhint %}

{% hint style="note" %}
eth_getLogs is disabled on below Mainnet endpoints, please use 3rd party endpoints from here. If you need to pull logs frequently, we recommend using WebSockets to push new logs to you when they are available.
{% endhint %}

## Mainnet (ChainID 0x29C, 668 in decimal)
- RPC endpoint: `https://rpc.juncachain.com`
- Websocket endpoint: `wss://ws.juncachain.com`

## Testnet (ChainID 0x29B, 667 in decimal)
- RPC endpoint: `https://rpc-testnet.juncachain.com`
- Websocket endpoint: `wss://ws-testnet.juncachain.com`

## Start HTTP JSON-RPC
You can start the HTTP JSON-RPC with the --http flag
```
## mainnet
junca attach https://rpc.juncachain.com

## testnet
junca attach https://rpc-testnet.juncachain.com
```

## JSON-RPC methods
Please refer to this wiki page or use Postman: https://documenter.getpostman.com/view/4117254/ethereum-json-rpc/RVu7CT5J?version=latest