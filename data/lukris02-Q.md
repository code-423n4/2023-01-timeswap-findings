# QA Report for Timeswap contest
## Overview

During the audit, 7 non-critical issues were found.

№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
NC-1 | Order of Functions | Non-Critical | 4
NC-2 | Order of Layout | Non-Critical | 1
NC-3 | Missing leading underscores | Non-Critical | 5
NC-4 | Unused named return variables | Non-Critical | 11
NC-5 | Missing NatSpec | Non-Critical | 23
NC-6 | Typos | Non-Critical | 81
NC-7 | Maximum line length exceeded | Non-Critical | 112

## Non-Critical Risk Findings(7)
### NC-1. Order of Functions
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-functions), ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.  
Functions should be grouped according to their visibility and ordered:
1) constructor
2) receive function (if exists)
3) fallback function (if exists)
4) external
5) public
6) internal
7) private
##### Instances
constructor should be placed before private functions:
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L77
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L65

external function should be placed before public function:
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L41

private functions should be placed after external and public functions:
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L45-L63

##### Recommendation
Reorder functions where possible.
#
### NC-2. Order of Layout
##### Description
According to [Order of Layout](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout), inside each contract, library or interface, use the following order:
1) Type declarations
2) State variables
3) Events
4) Modifiers
5) Functions
##### Instances
State variables should be placed before constructor:
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L41-L47

#
### NC-3. Missing leading underscores
##### Description
Private state variables should have a leading underscore.
##### Instances
- [```mapping(bytes32 => uint96) private reentrancyGuards;```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L41) 
- [```mapping(uint256 => mapping(uint256 => Option)) private options;```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L48) 
- [```Process[] private processing;```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L51) 
- [```mapping(uint256 => mapping(uint256 => bool)) private hasInteracted;```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L53) 
- [```StrikeAndMaturity[] private listOfOptions;```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L54) 

##### Recommendation
Add leading underscores.
#
### NC-4. Unused named return variables
##### Description
Both named return variable(s) and return statement are used.
##### Instances
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L89
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L119
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L124
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L129
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L249
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L314 
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L374
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L427
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L67 
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L76
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L84 

##### Recommendation
To improve clarity use only named return variables.  
For example, change:
```
function functionName() returns (uint id) {
    return x;
```
to
```
function functionName() returns (uint id) {
    id = x;
```
#
### NC-5. Missing NatSpec
##### Description
NatSpec is missing for 23 functions in 7 contracts.
##### Instances
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L12
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol#L25
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L65
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L69
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L75
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L79
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L85
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L49
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L49
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L236
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L255
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L259
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L263
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L45 
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L17
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L29
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L41
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L52
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L58
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L56
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L76
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L80
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L246

##### Recommendation
Add NatSpec for all functions.
#
### NC-6. Typos
##### Instances
- [```/// @param addendA0 The least signficant part of addendA.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L40) => ```significant```
- [```/// @dev When oneToZero, given a larger base amount, and toekn1 amount, get the difference token0 amount.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol#L40) => ```token1```
- [```/// @param to The receipeint of liquidity position.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L14) => ```recipient```
- [```/// @param to The receipeint of fees position.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L22) => ```recipient```
- [```/// @param long0To The receipient of long0 position fees.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L32) => ```recipient```
- [```/// @param long1To The receipient of long1 position fees.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L33) => ```recipient```
- [```/// @param shortTo The receipient of short position fees.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L34) => ```recipient```
- [```/// @param long0To The receipient of long0 position fees.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L54) => ```recipient```
- [```/// @param long1To The receipient of long1 position fees.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L55) => ```recipient```
- [```/// @param shortTo The receipient of short position fees.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L56) => ```recipient```
- [```/// @param to The receipient of liquidity positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L76) => ```recipient```
- [```/// @param long0To The receipient of long0 positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L87) => ```recipient```
- [```/// @param long1To The receipient of long1 positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L88) => ```recipient```
- [```/// @param shortTo The receipient of short positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L89) => ```recipient```
- [```/// @param to The receipient of short positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L111) => ```recipient```
- [```/// @param long0To The receipient of long0 positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L121) => ```recipient```
- [```/// @param long1To The receipient of long1 positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L122) => ```recipient```
- [```/// @param to If isLong0ToLong1 then receipient of long0 positions, ekse recipient of long1 positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L132) => ```else```
- [```/// @param to If isLong0ToLong1 then receipient of long0 positions, ekse recipient of long1 positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L132) => ```recipient```
- [```/// @param to The receipient of the liquidity positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L234) => ```recipient```
- [```/// @param to The receipient of the transaction fees.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L242) => ```recipient```
- [```/// @param long0Fees The amount of long0 position fees transferrred.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L243) => ```transferred```
- [```/// @param long1Fees The amount of long1 position fees transferrred.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L244) => ```transferred```
- [```/// @param shortFees The amount of short position fees transferrred.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L245) => ```transferred```
- [```/// @dev The short positions will already be minted to the receipient.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L10) => ```recipient```
- [```/// @dev The long0 positions and long1 positions will already be minted to the receipients.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L10) => ```recipient```
- [```/// @param long0Fees The amount of long0 position fees transferrred.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L179) => ```transferred```
- [```/// @param long1Fees The amount of long1 position fees transferrred.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L180) => ```transferred```
- [```/// @param shortFees The amount of short position fees transferrred.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L181) => ```transferred```
- [```/// @dev Reverts when there is not enough time value liqudity to receive when lending.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L19) => ```liquidity```
- [```/// @return sqrtDiscriminant The square root disriminant calculated.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L394) => ```discriminant```
- [```/// @return optionPair The retreived option pair address. Zero address if not deployed.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L27) => ```retrieved```
- [```/// @return poolPair The retreived pool pair address. Zero address if not deployed.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L28) => ```retrieved```
- [```/// @return optionPair The retreived option pair address.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L41) => ```retrieved```
- [```/// @return poolPair The retreived pool pair address.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L42) => ```retrieved```
- [```/// @dev Checks if the pool doesn not exist.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolPair.sol#L19) => ```does```
- [```/// @dev mints TimeswapV2LiquidityToken as per the liqudityAmount```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L44) => ```liquidityAmount```
- [```/// @dev burns TimeswapV2LiquidityToken as per the liqudityAmount```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L49) => ```liquidityAmount```
- [```/// @dev mints TimeswapV2Token as per postion and amount```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L27) => ```position```
- [```/// @dev burns TimeswapV2Token as per postion and amount```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L32) => ```position```
- [```/// @dev Any additional condition to add token enumeration when overidden.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L69) => ```overridden```
- [```/// @dev Any additional condition to add token enumeration when overidden.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L74) => ```overridden```
- [```/// @dev Any additional condition to remove token enumeration when overidden.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L103) => ```overridden```
- [```/// @dev Any additional condition to remove token enumeration when overidden.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L108) => ```overridden```
- [```console.log("reaches right before mint in timeswapv2Tokne::mint");```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L109) => ```timeswapv2Token```
- [```/// @dev paramater for minting Timeswap V2 Tokens```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L4) => ```parameter```
- [```/// @dev paramater for burning Timeswap V2 Tokens```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L24) => ```parameter```
- [```/// @param data Arbitrary data passed to the callback, initalize as empty if not required.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L32) => ```initialize```
- [```/// @dev paramater for minting Timeswap V2 Tokens```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L6) => ```parameter```
- [```/// @dev paramater for burning Timeswap V2 Tokens```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L32) => ```parameter```
- [```/// @param data Arbitrary data passed to the callback, initalize as empty if not required.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L43) => ```initialize```
- [```/// @dev paramater for minting Timeswap V2 Liquidity Tokens```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L58) => ```parameter```
- [```/// @dev paramater for burning Timeswap V2 Liquidity Tokens```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L76) => ```parameter```
- [```/// @param data Arbitrary data passed to the callback, initalize as empty if not required.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L83) => ```initialize```
- [```/// @dev paramater for collecting fees from Timeswap V2 Liquidity Tokens```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L94) => ```parameter```
- [```/// @param data Arbitrary data passed to the callback, initalize as empty if not required.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L103) => ```initialize```
- [```/// @param to The address of the receipient of the position.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L18) => ```recipient```
- [```/// @param long0To The address of the receipient of long token0 position.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L27) => ```recipient```
- [```/// @param long1To The address of the receipient of long token1 position.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L28) => ```recipient```
- [```/// @param shortTo The address of the receipient of short position.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L29) => ```recipient```
- [```/// @param token0To The address of the receipient of token0.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L49) => ```recipient```
- [```/// @param token1To The address of the receipient of token1.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L50) => ```recipient```
- [```/// @param tokenTo The address of the receipient of token0 or token1.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L69) => ```recipient```
- [```/// @param longTo The address of the receipient of long token0 or long token1.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L70) => ```recipient```
- [```/// @param token0To The address of the receipient of token0.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L91) => ```recipient```
- [```/// @param token1To The address of the receipient of token1.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L92) => ```recipient```
- [```/// @param to The address of the receipient of the position.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L145) => ```recipient```
- [```/// @dev The token0 and token1 will already transferred to the receipients.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol#L12) => ```recipients```
- [```/// @dev The long0 positions, long1 positions, and/or short positions will already minted to the receipients.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol#L12) => ```recipients```
- [```/// @param long0To The receipient of long0 positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L11) => ```recipient```
- [```/// @param long1To The receipient of long1 positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L12) => ```recipient```
- [```/// @param shortTo The receipient of short positions.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L13) => ```recipient```
- [```/// @param token0To The receipient of token0 withdrawn.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L35) => ```recipient```
- [```/// @param token1To The receipient of token1 withdrawn.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L36) => ```recipient```
- [```/// @notice If data length is zero, skips the calback.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L43) => ```callback```
- [```/// @param tokenTo The receipient of token0 when isLong0ToLong1 or token1 when isLong1ToLong0.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L58) => ```recipient```
- [```/// @param longTo The receipient of long1 positions when isLong0ToLong1 or long0 when isLong1ToLong0.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L59) => ```recipient```
- [```/// @param token0To The receipient of token0 withdrawn.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L81) => ```recipient```
- [```/// @param token1To The receipient of token1 withdrawn.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L82) => ```recipient```
- [```/// @notice If data length is zero, skips the calback.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L88) => ```callback```
- [```// Can be overidden for testing purposes.```](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L69) => ```overridden```

#
### NC-7. Maximum line length exceeded
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#maximum-line-length), maximum suggested line length is 120 characters.  Longer lines make the code harder to read.
##### Instances
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L46
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L62
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L68
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L115 
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L138
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L158
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L181
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol#L17
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol#L27
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol#L36
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L118
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L123
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L26
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L81
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L115
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L126
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L138
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L189
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L197
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L204
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L218
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L261
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L270
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L280
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L307
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L333
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L344
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L353
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L364
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L372
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol#L13
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L14
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L14
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L75
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L83
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L101
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L103
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L183
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L530
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L533
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L639
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L653
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L665
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L697
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L715
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L724
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L51
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L66
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L81
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L329
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L334
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L356
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L373
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L404
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L406
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L27
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L41
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L29
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L43
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L42
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L59
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L47
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L60
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L65
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L81
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L94
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L99
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L65
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L70
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L71
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L75
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L71
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L87
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L117
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L146
- (https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L186
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L189
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L192
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L205
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L210
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L213
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol#L8
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L29
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L21
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L130
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L138
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L159
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L169
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L182
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L190
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L33
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/Proportion.sol#L12
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L85
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L90
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L97
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L111
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L118
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L145
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L148
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L153
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L159
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L166
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L194
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L198
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L205
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L215
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L216
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L220
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L235
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L243
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L246

##### Recommendation
Make the lines shorter.