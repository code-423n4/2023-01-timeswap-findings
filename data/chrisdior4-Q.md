## [L-01] Check array arguments have the same length
In ERC1155Enumerable.sol:_beforeTokenTransfer you have multiple array-type arguments. Validate in both places that the arguments have the same length so you do not get unexpected errors if they don't.

## [NC-01]  Use latest Solidity version 
Use latest Solidity version to get all compiler features, bugfixes and optimizations

## [NC-02] Typos

File: `TimeswapV2Pool.sol`

receipient -> recipient

File: `Pool.sol`

transferrred -> transferred

File: `ConstantProduct.sol`

liqudity -> liquidity



## [NC-03] Constants should be defined rather than using magic numbers 

File: `ConstantProduct.sol`

```solidity
return FullMath.mulDiv(uint256(liquidity).unsafeMul(duration), uint256(rate), uint256(1) << 192, roundUp);
```

```solidity
return FullMath.mulDiv(uint256(rate), longAmount, uint256(1) << 96, roundUp).toUint160();
```
