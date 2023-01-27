
## 01 Typos

_There are 26 instances of this issue_

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-library/src/CatchError.sol

```
File: main/packages/v2-library/src/CatchError.sol

88: /// @dev It checks that the first four bytes of the reason has the same selector.
```

Typo: It should be `have the` instead of `has`

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-library/src/Error.sol

```
File: main/packages/v2-library/src/Error.sol

15: /// @dev Reverts when a pool already have liquidity.
81: /// @dev Reverts when a pool already have liquidity.
148: /// @dev Reverts when token amount not received.
```

Typo: In line 15 and 81, it should be `has` instead of `have`
In line 148, it should be `is not` instead of `not`

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-library/src/FullMath.sol

```
File: main/packages/v2-library/src/FullMath.sol

71: /// @dev Calculate the product of two uint256 numbers that may result to uint512 product.
```

Typo: It should be `result in a` instead of `result to`

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol

```
File: main/packages/v2-option/src/TimeswapV2Option.sol

31: /// @notice Holds the option of all strikes and maturies.
47: /// @dev mapping of all option state for all strikes and maturies.
```

Typo: In line 31 and 47, it should be `maturities` instead of `maturies`

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol

```
File: main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol

18: /// @param to The address of the receipient of the position.
145: /// @param to The address of the receipient of the position.
```

Typo: In line 18 and 145, it should be `recipient` instead of `receipient`

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol

```
File: main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol

12: /// @dev The token0 and token1 will already transferred to the receipients.
```

Typo: It should be `recipients` instead of `receipients`

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol

```
File: main/packages/v2-pool/src/structs/Param.sol

35: /// @param token0To The receipient of token0 withdrawn.
36: /// @param token1To The receipient of token1 withdrawn.
43: /// @notice If data length is zero, skips the calback.
58: /// @param tokenTo The receipient of token0 when isLong0ToLong1 or token1 when isLong1ToLong0.
59: /// @param longTo The receipient of long1 positions when isLong0ToLong1 or long0 when isLong1ToLong0.
81: /// @param token0To The receipient of token0 withdrawn.
82: /// @param token1To The receipient of token1 withdrawn.
```

Typo: In lines 35, 36, 58, 59, 81 and 82 and 81, it should be `recipient` instead of `receipient`
In line 43, it should be `callback` instead of `calback`

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol

```
File: main/packages/v2-pool/src/TimeswapV2Pool.sol

222: /// @dev Transfer long0 positions, long1 positions, and/or short positions to the receipients
225: /// @param long0To The receipient of long0 positions.
226: /// @param long1To The receipient of long1 positions.
227: /// @param shortTo The receipient of short positions.
399: // Transfer short positions to the receipient.
446: // Transfer the positions to the receipients.
486: // Transfer the positions to the receipients.
```

Typo: In lines 225, 226, 227, and 399, it should be `recipient` instead of `receipient`
In line 222, 446 and 486, it should be `recipients` instead of `receipients`

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/libraries/PoolPair.sol

```
File: main/packages/v2-pool/src/libraries/PoolPair.sol

8: /// @dev Reverts when the Timeswap V2 Pool already exist.
19: /// @dev Checks if the pool doesn not exist.
```

Typo: In lines 8, it should be `exists` instead of `exist`
In line 19, it should be `doesn't` instead of `doesn not

---

## 02 Using both named returns and a return statement isn't necessary 

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

_There are 15 instances of this issue_

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-library/src/Math.sol

```
File: main/packages/v2-library/src/Math.sol

88: function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol

```
File: main/packages/v2-pool/src/TimeswapV2Pool.sol

118: function feeGrowth(uint256 strike, uint256 maturity) external view override returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {

123: function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view override 
returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

128: function protocolFeesEarned(uint256 strike, uint256 maturity) external view override returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {

240: function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

245-248: function mint(
        TimeswapV2PoolMintParam calldata param,
        uint96 durationForward
    ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

305: function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

310-313: function burn(
        TimeswapV2PoolBurnParam calldata param,
        uint96 durationForward
    ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

365: function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

370-373: function deleverage(
        TimeswapV2PoolDeleverageParam calldata param,
        uint96 durationForward
    ) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

421: function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

426: function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol

```
File: main/packages/v2-pool/src/structs/Pool.sol

66: function feeGrowth(Pool storage pool) external view returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {

75: function feesEarnedOf(Pool storage pool, address owner) external view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

83: function protocolFeesEarned(Pool storage pool) external view returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {
```

---------

## 03 Don't use periods with fragments

Throughout the files, most of the comments have fragments that end with periods. They should either be converted to actual sentences with both a noun phrase and a verb phrase, or the periods should be removed.

_There are multiple instances of this issue. This exists in all contracts_

For instance,

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/libraries/PoolPair.sol

```
File: main/packages/v2-pool/src/libraries/PoolPair.sol

5: /// @dev Reverts when swap address is zero.
8: /// @dev Reverts when the Timeswap V2 Pool already exist.
10: /// @param poolPair The address of the existed Timeswap V2 Pair contract.
13: /// @dev Checks if option address is not zero.
14: /// @param poolPair The address being checked upon.
19: /// @dev Checks if the pool doesn not exist.
20: /// @param optionPair The address of the TimeswapV2Option contract.
21: /// @param poolPair The address of the TimeswapV2Pool contract.
```

-------

## 04 Use a more recent version of solidity

It's a best practice to use the latest compiler version.

The specified minimum compiler version is quite old. Older compilers might be susceptible to some bugs. We recommend changing the solidity version pragma to the latest version to enforce the use of an up to date compiler.

List of known compiler bugs and their severity can be found here: [https://etherscan.io/solcbuginfo](https://etherscan.io/solcbuginfo)

This issue exists in all the In-scope contracts

----------

## 05 Non-library or interface files should use fixed compiler versions, not floating ones

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

_There is 1 instance of this issue_

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol

```
File: main/packages/v2-pool/src/libraries/PoolPair.sol

2: pragma solidity ^0.8.8; 
```

-----------


