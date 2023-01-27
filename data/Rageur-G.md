## GAS-1: ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow

### Description

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

### Affected file

* StdStorage.sol (Line: 63, 63, 63, 169, 169, 169, 177, 177, 177, 299, 299, 299, 308, 308, 308)

## GAS-2: <X> * 2 costs more gas then <X> << 1

### Description

Using bitwize operator instead of '*' or '/' saves gas.

### Affected file

* StdStorage.sol (Line: 170, 170, 170, 176, 176, 176, 300, 300, 300, 307, 307, 307)

## GAS-3: <X> <= <Y> costs more gas than <X> < <Y> + 1

### Description

In Solididy, the opcode 'less or equal' doesn't exist. So the EVM will translate this by two distinct operation, first the inferior, and then the equal which cost more gas then a strict less.

### Affected file

* BytesLib.sol (Line: 13, 14)
* ConstantProduct.sol (Line: 108, 288, 298, 411)
* FullMath.sol (Line: 189)
* LiquidityPosition.sol (Line: 91, 99, 107)
* OptionPair.sol (Line: 30)
* Param.sol (Line: 106, 120, 133)
* Pool.sol (Line: 226, 234, 242)
* Position.sol (Line: 23)

## GAS-7: Internal functions not called by the contract should be removed to save deployment gas

### Description

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword.

### Affected file

* BytesLib.sol (Line: 12)
* CatchError.sol (Line: 12)
* ConstantProduct.sol (Line: 51, 66, 81, 98, 134, 164, 195, 230)
* ConstantSum.sol (Line: 19, 30, 39)
* Duration.sol (Line: 11)
* DurationCalculation.sol (Line: 18, 26, 34, 43)
* DurationWeight.sol (Line: 15)
* Error.sol (Line: 67, 72, 77, 83, 88, 93, 99, 106, 113, 119, 125, 132, 139, 144, 151)
* Fee.sol (Line: 10)
* FeeCalculation.sol (Line: 27, 40, 47, 54, 68)
* FeesPosition.sol (Line: 17, 29, 52, 58)
* FullMath.sol (Line: 62, 181, 265)
* LiquidityPosition.sol (Line: 33, 51, 65, 69, 75, 79, 85)
* Math.sol (Line: 14, 25, 36, 48, 59, 69, 88)
* Option.sol (Line: 38, 49, 60, 85, 127, 156, 191)
* OptionFactory.sol (Line: 18, 37)
* OptionPair.sol (Line: 21, 29, 38)
* Ownership.sol (Line: 22, 30, 38)
* Param.sol (Line: 118, 125, 132, 139, 146)
* ParamExt.sol (Line: 7, 11, 15, 19)
* PoolFactory.sol (Line: 18)
* PoolPair.sol (Line: 15, 22)
* Position.sol (Line: 22, 34, 39)
* Process.sol (Line: 33)
* Proportion.sol (Line: 12)
* ReentrancyGuard.sol (Line: 23)
* SafeCast.sol (Line: 18, 27, 36)
* StdMath.sol (Line: 31, 31, 31, 37, 37, 37)
* StdStorage.sol (Line: 101, 101, 101, 106, 106, 106, 111, 111, 111, 116, 116, 116, 121, 121, 121, 126, 126, 126, 131, 131, 131, 142, 142, 142, 153, 153, 153, 157, 157, 157)
* StrikeConversion.sol (Line: 26, 35, 46)
* TimeswapV2LiquidityToken.sol (Line: 255, 259)
* TimeswapV2OptionDeployer.sol (Line: 32)
* TimeswapV2PoolDeployer.sol (Line: 25)

## GAS-8: Internal functions only called once can be inlined to save gas

### Description

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

### Affected file

* FeeCalculation.sol (Line: 61, 75)
* PoolFactory.sol (Line: 43)
* StdStorage.sol (Line: 22, 22, 22, 146, 146, 146, 161, 161, 161, 192, 192, 192, 200, 200, 200, 224, 224, 224, 274, 274, 274, 278, 278, 278, 282, 282, 282, 286, 286, 286, 290, 290, 290)
* TimeswapV2LiquidityToken.sol (Line: 224)

## GAS-9: Length calculated at each iteration in For-loop

### Description

Caching the array length outside a loop saves reading it on each iteration, as long as the array’s length is not changed during the loop.

### Affected file

* Process.sol (Line: 34)
* StdStorage.sol (Line: 63, 63, 63, 177, 177, 177, 308, 308, 308)
* TimeswapV2LiquidityToken.sol (Line: 227)

## GAS-11: Not using the named return variables when a function returns, wastes deployment gas

### Description

It is not necessary to have both a named return and a return statement.

### Affected file

* ConstantProductExt.sol (Line: 15, 19, 23, 27, 38, 49, 60, 71)
* FullMathExt.sol (Line: 7, 11, 15, 31, 35, 39, 43)
* Math.sol (Line: 88)
* MathExt.sol (Line: 7, 11, 15, 19, 23, 27, 31)
* Pool.sol (Line: 66, 75, 83)
* SafeCastExt.sol (Line: 6, 10, 14)
* StdStorage.sol (Line: 101, 101, 101, 106, 106, 106, 111, 111, 111, 116, 116, 116, 121, 121, 121, 126, 126, 126, 131, 131, 131, 200, 200, 200, 204, 204, 204, 208, 208, 208, 212, 212, 212, 216, 216, 216, 220, 220, 220, 224, 224, 224)
* TimeswapV2Pool.sol (Line: 118, 123, 128, 240, 248, 305, 313, 365, 373, 421, 426)

## GAS-12: Prefix increments cheaper than Postfix increments

### Description

++i costs less gas than i++, especially when it’s used in for-loops (—i/i— too). Saves 5 gas per loop.

### Affected file

* FullMath.sol (Line: 146, 167, 168, 257, 277)
* Math.sol (Line: 51, 62, 81)
* Process.sol (Line: 42)
* StdStorage.sol (Line: 63, 63, 63, 169, 169, 169, 177, 177, 177, 299, 299, 299, 308, 308, 308)

## GAS-13: Public functions not called by the contract should be declared external instead

### Description

Contracts are allowed to override their parents’ functions and change the visibility from external to public and can save gas by doing so. 

### Affected file

* ProcessExt.sol (Line: 4)

## GAS-14: Replace modifier with function

### Description

Modifiers make code more elegant, but cost more than normal functions.

### Affected file

* NoDelegateCall.sol (Line: 34, 34)

## GAS-15: Require()/Revert() strings longer than 32 bytes cost extra gas

### Description

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas.

### Affected file

* StdStorage.sol (Line: 88, 150, 150, 150)

## GAS-16: Should use arguments instead of state variable

### Description

Using function's parameter cost less gas then a state variable.

### Affected file

* OwnableTwoSteps.sol (Line: 31)

## GAS-19: Use custom errors rather than revert()/require() strings to save gas

### Description

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas.

### Affected file

* BytesLib.sol (Line: 13, 14)
* StdStorage.sol (Line: 57, 57, 57, 88, 88, 88, 91, 91, 91, 150, 150, 150, 265, 265, 265)

## GAS-22: Using Solidity version 0.8.17 will provide an overall gas optimization

### Description

Using at least 0.8.10 will save gas due to skipped extcodesize check if there is a return value.

### Affected file

* BytesLib.sol (Line: 2)
* BytesLibExt.sol (Line: 3)
* CallbackParam.sol (Line: 2, 2, 2)
* CatchError.sol (Line: 2)
* CatchErrorExt.sol (Line: 2)
* ConstantProduct.sol (Line: 2)
* ConstantProductExt.sol (Line: 2)
* ConstantSum.sol (Line: 2)
* Duration.sol (Line: 2)
* DurationCalculation.sol (Line: 2)
* DurationCalculationExt.sol (Line: 2)
* DurationExt.sol (Line: 2)
* DurationWeight.sol (Line: 2)
* DurationWeightExt.sol (Line: 2)
* ERC1155Enumerable.sol (Line: 4)
* Error.sol (Line: 2)
* ErrorExt.sol (Line: 2)
* Fee.sol (Line: 2)
* FeeCalculation.sol (Line: 2)
* FeeExt.sol (Line: 2)
* FeesPosition.sol (Line: 2)
* FullMath.sol (Line: 2)
* FullMathExt.sol (Line: 2)
* IERC1155Enumerable.sol (Line: 4)
* IOwnableTwoSteps.sol (Line: 2)
* ITimeswapV2LiquidityToken.sol (Line: 2)
* ITimeswapV2LiquidityTokenBurnCallback.sol (Line: 2)
* ITimeswapV2LiquidityTokenCollectCallback.sol (Line: 2)
* ITimeswapV2LiquidityTokenMintCallback.sol (Line: 2)
* ITimeswapV2Option.sol (Line: 2)
* ITimeswapV2OptionBurnCallback.sol (Line: 2)
* ITimeswapV2OptionCollectCallback.sol (Line: 2)
* ITimeswapV2OptionDeployer.sol (Line: 2)
* ITimeswapV2OptionFactory.sol (Line: 2)
* ITimeswapV2OptionMintCallback.sol (Line: 2)
* ITimeswapV2OptionSwapCallback.sol (Line: 2)
* ITimeswapV2Pool.sol (Line: 2)
* ITimeswapV2PoolBurnCallback.sol (Line: 2)
* ITimeswapV2PoolDeleverageCallback.sol (Line: 2)
* ITimeswapV2PoolDeployer.sol (Line: 2)
* ITimeswapV2PoolFactory.sol (Line: 2)
* ITimeswapV2PoolLeverageCallback.sol (Line: 2)
* ITimeswapV2PoolMintCallback.sol (Line: 2)
* ITimeswapV2PoolRebalanceCallback.sol (Line: 2)
* ITimeswapV2Token.sol (Line: 2)
* ITimeswapV2TokenBurnCallback.sol (Line: 2)
* ITimeswapV2TokenMintCallback.sol (Line: 2)
* LiquidityPosition.sol (Line: 2)
* Math.sol (Line: 2)
* MathExt.sol (Line: 3)
* NoDelegateCall.sol (Line: 2, 2)
* Option.sol (Line: 2)
* OptionFactory.sol (Line: 2)
* OptionFactoryExt.sol (Line: 3)
* OptionPair.sol (Line: 2)
* OptionPairExt.sol (Line: 2)
* OwnableTwoSteps.sol (Line: 2)
* Ownership.sol (Line: 2)
* Param.sol (Line: 2, 2, 2)
* ParamExt.sol (Line: 2)
* Pool.sol (Line: 2)
* PoolFactory.sol (Line: 2)
* PoolFactoryExt.sol (Line: 2)
* PoolPair.sol (Line: 2)
* PoolPairExt.sol (Line: 2)
* Position.sol (Line: 2, 2)
* Process.sol (Line: 2)
* Proportion.sol (Line: 2)
* ProportionExt.sol (Line: 2)
* ReentrancyGuard.sol (Line: 2)
* SafeCast.sol (Line: 2)
* SafeCastExt.sol (Line: 2)
* StrikeAndMaturity.sol (Line: 2)
* StrikeConversion.sol (Line: 2)
* StrikeConversionExt.sol (Line: 2)
* TimeswapV2LiquidityToken.sol (Line: 2)
* TimeswapV2Option.sol (Line: 2)
* TimeswapV2OptionDeployer.sol (Line: 2)
* TimeswapV2OptionFactory.sol (Line: 2)
* TimeswapV2Pool.sol (Line: 2)
* TimeswapV2PoolDeployer.sol (Line: 2)
* TimeswapV2PoolFactory.sol (Line: 2)
* TimeswapV2Token.sol (Line: 2)
* Transaction.sol (Line: 2, 2)

## GAS-23: Using bools for storage incurs overhead

### Description

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot’s contents, replace the bits taken up by the boolean, and then write back. This is the compiler’s defense against contract upgrades and pointer aliasing, and it cannot be disabled. Use uint256(1) and uint256(2) for true/false instead.

### Affected file

* TimeswapV2Option.sol (Line: 53)

## GAS-25: Variable initialized with their default value

### Description

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it’s default value costs unnecesary gas.

### Affected file

* ReentrancyGuard.sol (Line: 12)
* StdStorage.sol (Line: 63, 169, 177, 299, 308)

## GAS-26: abi.encode() is less efficient than abi.encodePacked()

### Description

Use abi.encodePacked() where possible to save gas.

### Affected file

* Position.sol (Line: 35, 40)
* StdStorage.sol (Line: 139, 139, 139)
* TimeswapV2OptionDeployer.sol (Line: 35)
* TimeswapV2PoolDeployer.sol (Line: 28)
