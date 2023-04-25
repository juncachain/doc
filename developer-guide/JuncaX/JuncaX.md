# Junca X protocol
It is a secure and efficient permissionless decentralized exchange (DEX) protocol that empowers a diverse system of DEXs, MM providers, and independent projects to work together in a decentralized manner.

IJuncaswapV2Pair is an extension of IUniswapV2Pair：
```
interface IJuncaswapV2Pair {
	...
	// whether gasless pair or not
	function gasless()external view returns(bool);
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

## Feature
- Support uniswap-like transactions, you can use uniswap interface to access and trade on Juncachain
- The transaction fee is 0.3% of the sold token
- Support the creation of `GasLessPair`, users do not need gas fees to trade in GasLessPair, and even complete the transaction without holding JGC
- JuncaswapRouter1: Transactions via JuncaswapRouter1 cost gas, and the transaction fee is 0.3%, of which 0.25% is rewarded to liquidity providers and 0.05% is rewarded to the platform, which is exactly the same as uniswap
- JuncaswapRouter2: `GasLessPair` can only be traded through JuncaswapRouter2, the transaction fee is 0.3%, of which 0.15% is rewarded to liquidity providers, 0.05% is rewarded to the platform, and 0.1% is rewarded to the creator of the pair
- Create `GasLessPair`: call `JuncaswapFactory.createPair` to create a gasless transaction pair (GasLess)
- Set the minimum transaction volume: `GasLessPair.setMTV`
- Recharge gas: GasLessPair.gasRecharge, if the gas in the transaction pair is lower than the low water mark, the recharger will win the issuer status and get transaction fees
- Query the miner fee to be paid for each swap transaction: `GasLessPair.gasPerTx`, this value is dynamically adjusted by the platform according to network congestion
- Query the remaining amount of gas in the `GasLessPair`: `GasLessPair.gasLeft`, when gasLeft is less than gasPerTx, the transaction will not be completed
- Query the gas low water mark of the `GasLessPair`: `GasLessPair.gasLowWaterMark`, when the gas is lower than the low water mark, the issuer may be contested

## Contract Address
- JuncaswapWJGC: `0x000000000000004a756e636153776170574A4743`
- JuncaswapFactory: `0x000000004a756e636173776170466163746F7279`
- JuncaswapRouter1: `0x000000004A756E636173776170526f7574657231`
- JuncaswapRouter2: `0x000000004a756E636173776170526F7574657232`