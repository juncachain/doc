# Junca X protocol
It is a secure and efficient permissionless decentralized exchange (DEX) protocol that empowers a diverse system of DEXs, MM providers, and independent projects to work together in a decentralized manner.

IJuncaswapV2Pair is an extension of IUniswapV2Pair：
```
interface IJuncaswapV2Pair {
	...
	// whether nogas pair or not
	function nogas()external view returns(bool);
	// gas left to pay for minerfee
	function gasLeft()external view returns(uint);
	// pay for minerfee per transaction
    function gasPerTx()external view returns(uint);
	// the gas low water mark
    function gasLowWaterMark()external returns(uint);
	// pair creater
	function issuer()external view returns(address);
	// recharge gas to pay for minerfee,anyone can recharge to obtain the authority to fees
	// min recharge value is 1 ether JGC
	function gasRecharge()external payable;
	// return the min sell value of token0 and token1
    function mtv()external view returns(uint,uint);
	// set min tranact value,only by issuer
	function setMTV(uint _mtv0,uint _mtv1)external;
}
```

## feature
- Support uniswap-like transactions, you can use uniswap interface to access and trade on juncachain
- The transaction fee is 0.3% of the sold token
- Support the creation of `nogaspair`, users do not need gas fees to trade in nogaspair, and even complete the transaction without holding JGC
- JuncaswapRouter1: Transactions via JuncaswapRouter1 cost gas, and the transaction fee is 0.3%, of which 0.25% is rewarded to liquidity providers and 0.05% is rewarded to the platform, which is exactly the same as uniswap
- JuncaswapRouter2: `nogaspair` can only be traded through JuncaswapRouter2, the transaction fee is 0.3%, of which 0.15% is rewarded to liquidity providers, 0.05% is rewarded to the platform, and 0.1% is rewarded to the creator of the pair
- Create `nogaspair`: call `JuncaswapFactory.createPair` to create a nogas transaction pair (NOGASPAIR)
- Set the minimum transaction volume: `NOGASPAIR.setMTV`
- 充值gas:`NOGASPAIR.gasRecharge`，如果交易对中gas处于低水位线，充值者赢得issuer身份并获得后续交易手续费
- Recharge gas: NOGASPAIR.gasRecharge, if the gas in the transaction pair is lower than the low water mark, the recharger will win the issuer status and get transaction fees
- Query the miner fee to be paid for each swap transaction: `NOGASPAIR.gasPerTx`, this value is dynamically adjusted by the platform according to network congestion
- Query the remaining amount of gas in the `nogaspair`: `NOGASPAIR.gasLeft`, when gasLeft is less than gasPerTx, the transaction will not be completed
- Query the gas low water mark of the `nogaspair`: `NOGASPAIR.gasLowWaterMark`, when the gas is lower than the low water mark, the issuer may be contested