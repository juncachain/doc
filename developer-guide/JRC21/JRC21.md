# How to integrate JRC21
JRC21, powered by JuncaZ Protocol, creates a frictionless experience for non-crypto users by allowing token holders to pay transaction fees by the token itself without having to hold any JUNCA in their wallet.

JRC21 tokens are expanded on the basis of JRC20 tokens. The JRC21 contract contains all the methods of JRC20. The extension interface is:
```
interface IJRC21{
	// return the JRC21_ISSUER_CONTRACT
    function issuer() external view returns(address);
	// estimate JRC21 transfer fee
    function estimateFee(uint256 value) external view returns (uint256);
	// set the min JRC21 transfer fee,only JRC21 issuer
    function setMinFee(uint256 value)external;
}
```

Two types of JRC21 tokens are currently supported, namely:
- `JRC21PresetFixed` Fixed supplied token 
- `JRC21PresetMinter` Tokens that can be minted 

The JRC21Issuer contract(JRC21_ISSUER_CONTRACT) is a built-in contract,contract address is `0x0000000000000000004A52433231497373756572`

## Principle
- Anyone can issue JRC21 tokens through the JRC21_ISSUER_CONTRACT and need to deposit 50000 ether for gas；
- JRC21 itself satisfies all interfaces of JRC20 and can be directly used as JRC20；
- When the JRC21_ISSUER_CONTRACT is called to operate the JRC21 transfer, the prepaid GAS will be used to pay the miners；
- Call-Path：USER->JRC21_ISSUER_CONTRACT->JRC21
- Pay-Path：JRC21_ISSUER_CONTRACT--[GAS]-->coinbase from--[JRC21]--[to] from--[JRC21 FEE]--[JRC21 issuer]

## Issue
When issuing, you need to deposit 50,000 ether into the issuing contract as a prepaid GAS for the transaction.
- Call the JRC21_ISSUER_CONTRACT.issueJRC21PresetFixed method
- Call the JRC21_ISSUER_CONTRACT.issueJRC21PresetMinter method
- Query transaction results, get JRC21 token contract address from event,event name is `Issue`

## Call

### Determine whether it is a JRC21 token contract
- call JRC21_ISSUER_CONTRACT.issuer() method , return the JRC21 Issuer

### Gas Recharge
When the GAS of the JRC21 contract is exhausted, no GAS-free transactions can be performed, and anyone can recharge the tokens with GAS
- call JRC21_ISSUER_CONTRACT.charge(address token) public payable # recharge
- call JRC21_ISSUER_CONTRACT.gasLeft(address token)public view returns(uint256) # return the gas left

### Permit+Transfer（recommend）
This mode is offline signature authorization + transfer as a transaction, the principle is that the user authorizes JRC21_ISSUER_CONTRACT to spend the user's assets. Permit amount: value+fee
- call JRC21_ISSUER_CONTRACT.transferWithPermit(address token,address recipient,uint256 value,uint256 fee,uint deadline, uint8 v, bytes32 r, bytes32 s)

### Approve and Transfer
This mode is that the user first authorizes JRC21_ISSUER_CONTRACT to spend the user's assets, and then initiates the transfer transaction
- call JRC21.approve(address spender, uint256 amount) external returns (bool)
- call JRC21.allowance(address owner, address spender) external view returns (uint256)
- call JRC21_ISSUER_CONTRACT.transfer(address token,address recipient,uint256 value,uint256 fee)external

### Contract ABI
- [JRC21Issuer](JRC21Issuer.abi)
- [IJRC21](IJRC21.abi)
- [JRC21](JRC21.abi)