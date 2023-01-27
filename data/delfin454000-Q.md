## Summary	
### Low risk findings

| Issue | Description | Instances |
| -- | ----------- | -------- |
|1| Duplicate file names could be problematic|5|
|| Total|5|

### Non-critical findings

| Issue | Description | Instances |
| -- | ----------- | -------- |
|1| Event is missing `indexed` fields|3|
|2| Long single-line comments|51|
|3| Typos|97|
|4| `pragma solidity` version should be upgraded to latest version before finalization|3|
|5-1| Natspec completely or mostly missing for `function`|8|
|5-2| `@param` missing for `function`|3|
|5-3| `@return` missing for `function`|30|
|5-4| Natspec is missing for `event`|1|
|| Total|196|

### Low risk findings

| No. | Explanation + cases | 
|--| ----------- | 
|1|**Duplicate file names could be problematic**|
|  |The in-scope contacts include multiple instances of duplicate file names, as shown below|
|  |The existence of two or more files with in same name within the `packages` folder could cause errors and confusion, and could also cause compilation problems under some circumstances|

**CallbackParam.sol**

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/CallbackParam.sol

**NoDelegateCall.sol**

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/NoDelegateCall.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/NoDelegateCall.sol

**Param.sol**

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol

**Position.sol**

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Position.sol

**Transaction.sol**

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol

___
___



### Non-critical findings

| No. | Description |
|--| ----------- | 
|1|**Event is missing `indexed` fields**|
|  |Each `event` should use three `indexed` fields if there are three or more fields; if fewer, all should be indexed, assuming gas cost is not too expensive|

___
[IOwnableTwoSteps.sol: L5-7](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol#L5-L7)
```solidity
    /// @dev Emits when the pending owner is chosen.
    /// @param pendingOwner The new pending owner.
    event SetOwner(address pendingOwner);
```
___
[IOwnableTwoSteps.sol: L9-11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol#L9-L11)
```solidity
    /// @dev Emits when the pending owner accepted and become the new owner.
    /// @param owner The new owner.
    event AcceptOwner(address owner);
```
___
[ITimeswapV2LiquidityToken.sol: L13](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L13)
```solidity
    event TransferFees(address from, address to, TimeswapV2LiquidityTokenPosition position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees);
```
___
___

| No. | Explanation + cases | 
|--| ------------------------------------------------------------------------------------------- | 
|2|**Long single-line comments**|
|  |In theory, comments over 80 characters should wrap using multi-line comment syntax. Even if longer comments (up to 120 characters) are becoming acceptable and a scroll bar is provided, very long comments can interfere with readability|
|  |Below are five instances of extra-long comments (over 120 characters) whose readability could be improved by wrapping, as shown. Note that a small indentation is used in each continuation line (which flags for the reader that the comment is ongoing), along with a period at the close (to signal the end of the comment)|

___
[v2-option/src/structs/Param.sol: L62](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L62)
```solidity
/// @param amount If isLong0ToLong1 and transaction is GivenToken0AndLong0, this is the amount of token0 withdrawn, and the amount of long0 position burnt.
```
Suggestion:
```solidity
/// @param amount If isLong0ToLong1 and transaction is GivenToken0AndLong0, this is the 
///    amount of token0 withdrawn, and the amount of long0 position burnt.
```
___
[v2-option/src/structs/Param.sol: L63](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L63)
```solidity
/// If isLong1ToLong0 and transaction is GivenToken0AndLong0, this is the amount of token0 to be deposited, and the amount of long0 position minted.
```
Suggestion:
```solidity
/// If isLong1ToLong0 and transaction is GivenToken0AndLong0, this is the amount of
///   token0 to be deposited, and the amount of long0 position minted.
```
___
[ConstantProduct.sol: L57](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L57)
```solidity
    /// @dev Calculate the amount of liquidity positions and amount of short positions from given long positions in base denomination.
```
Suggestion:
```solidity
    /// @dev Calculate the amount of liquidity positions and amount of long positions 
    ///   in base denomination or short position whichever is larger or smaller.
```
___
[Pool.sol: L106](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L106)
```solidity
    /// @dev Move short positions to short fee growth due to duration of the pool decreasing as time moves forward.
```
Suggestion:
```solidity
    /// @dev Move short positions to short fee growth due to duration of the
    ///   pool decreasing as time moves forward.
```
___
[v2-pool/src/structs/Param.sol: L76](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L76)
```solidity
/// If transaction is  GivenSum, the sum amount of long position in base denomination to be deposited, and short position to be withdrawn.
```
Suggestion:
```solidity
/// If transaction is  GivenSum, the sum amount of long position in base
///   denomination to be deposited, and short position to be withdrawn.
```
___
Similarly for the following extra-long (over 120 characters) comments: 

**StrikeConversion.sol**

[L7](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/StrikeConversion.sol#L7)

**ITimeswapV2Pool.sol**

[L170](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L170), [L256](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L256), [L265](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L265), [L299](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L299), [L300](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L300), [L312](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L312)

**ITimeswapV2PoolBurnCallback.sol**

[L9](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol#L9)

**ITimeswapV2PoolLeverageCallback.sol**

[L9](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L9)

**ITimeswapV2PoolMintCallback.sol**

[L9](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L9)

**ITimeswapV2PoolDeleverageCallback.sol**

[L9](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L9)

**v2-pool/src/structs/Param.sol**

[L96](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L96)

**Pool.sol**

[L139](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L139), [L212](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L212), [L253](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L253), [L370](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L370), [L371](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L371)

**ConstantProduct.sol**

[L57](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L57), [L61](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L61), [L128](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L128), [L191](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L191)

[L216](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L216), [L224](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L224), [L328](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L328), [L341](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L341) 

**DurationCalculation.sol**

[L38](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L38), [L42](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L42)

**ITimeswapV2Option.sol**

[L72](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L72), [L74](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L74)

**ITimeswapV2OptionSwapCallback.sol**

[L11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol#L11)

**ITimeswapV2OptionMintCallback.sol**

[L11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol#L11)

**ITimeswapV2OptionCollectCallback.sol**

[L11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol#L11)

**ITimeswapV2OptionBurnCallback.sol**

[L10](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol#L10), [L11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol#L11)

**Option.sol**

[L19](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L19)

**v2-option/src/structs/Param.sol**

[L15](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L15), [L16](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L16), [L17](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L17), [L18](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L18), [L38](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L38), [L39](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L39)

[L40](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L40), [L41](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L41), [L60](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L60), [L64](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L64), [L65](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L65)
___
___


| No. | Description |
|--| ----------- | 
|3|**Typos**|

___
[FullMath.sol: L40](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L40)
```solidity
    /// @param addendA0 The least signficant part of addendA.
```
Change `signficant` to `significant`
___
[FullMath.sol: L250](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L250)
```solidity
            // correct result modulo 2**256. Since the precoditions guarantee
```
Change `precoditions` to `preconditions`
___
The same typo (`have`) occurs in both lines below:

[Error.sol: L15](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Error.sol#L15)

[Error.sol: L81](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Error.sol#L81)
```solidity
    /// @dev Reverts when a pool already have liquidity.
```
Change `have` to `has` in both cases
___
[StrikeConversion.sol: L22](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/StrikeConversion.sol#L22)
```solidity
    /// @param amount The amount ot be converted. Token0 amount when zeroToOne. Token1 amount when oneToZero.
```
Change `ot` to `to`
___
The same typo (`overidden`) occurs in all six lines below:

[TimeswapV2Pool.sol: L81](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L81)

[ERC1155Enumerable.sol: L69](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L69)

[ERC1155Enumerable.sol: L74](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L74)

[ERC1155Enumerable.sol: L103](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L103)

[ERC1155Enumerable.sol: L108](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L108)

[TimeswapV2Option.sol: L69](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L69)
```solidity
    // Can be overidden for testing purposes.
```
Change `overidden` to `overridden` in all cases
___
[v2-token/src/structs/CallbackParam.sol: L32](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/CallbackParam.sol#L32)
```solidity
/// @param data Arbitrary data passed to the callback, initalize as empty if not required.
```
Change `initalize` to `initialize`
___
The same typo (`receipients`) occurs in all seven lines below:

[TimeswapV2Pool.sol: L222](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L222)

[TimeswapV2Pool.sol: L332](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L332)

[TimeswapV2Pool.sol: L446](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L446)

[TimeswapV2Pool.sol: L486](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L486)

[ITimeswapV2OptionMintCallback.sol: L12](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol#L12)

[ITimeswapV2OptionCollectCallback.sol: L12](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol#L12)

[ITimeswapV2OptionBurnCallback.sol: L12](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol#L12)
```solidity
    /// @dev Transfer long0 positions, long1 positions, and/or short positions to the receipients.
```
Change `receipients` to `recipients` in all cases
___
The same typo (`receipient`) occurs in all 54 lines below:

***TimeswapV2Pool.sol***

[L225](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L225), [L226](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L226), [L227](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L227), [L399](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L399)

[IL32](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L32), [IL33](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L33), [IL34](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L34), [IL76](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L76)

[IL87](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L87), [IL88](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L88), [IL89](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L89), [IL111](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L111)

[IL121](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L121), [IL122](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L122), [IL132](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L132), [IL234](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L234), [IL242](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L242)

***ITimeswapV2PoolRebalanceCallback.sol***

[L10](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol#L10)

***ITimeswapV2PoolMintCallback.sol***

[L10](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L10)

***ITimeswapV2PoolDeleverageCallback.sol***

[L10](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L10)

***v2-pool/src/structs/Param.sol***

[L11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L11), [L12](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L12), [L13](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L13), [L31](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L31)

[L49](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L49), [L50](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L50), [L51](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L51), [L71](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L71)

[L90](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L90), [L91](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L91), [L112](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L112)

***Pool.sol***

[L155](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L155), [L168](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L168), [L178](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L178)

***ITimeswapV2Option***

[L18](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L18), [L27](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L27), [L28](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L28), [L29](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L29)

[L49](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L49), [L50](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L50), [L69](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L69), [L70](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L70)

[L91](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L91), [L92](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L92), [L145](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L145)

***v2-option/src/structs/Param.sol***

[L11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L11), [L12](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L12), [L13](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L13), [L35](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L35), [L36](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L36)

[L58](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L58), [L59](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L59), [L81](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L81), [L82](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L82)

```solidity
    /// @param long0To The receipient of long0 positions.
```
Change `receipient` to `recipient` in all cases
___
The same typo (`receipeint`) occurs in all three lines below:

[ITimeswapV2Pool.sol: L14](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L14)

[ITimeswapV2Pool.sol: L22](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L22)

[TimeswapV2Option.sol: L69](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L69)
```solidity
    /// @param to The receipeint of liquidity position.
```
Change `receipeint` to `recipient` in each case
___
[ITimeswapV2Pool.sol: L132](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L132)
```solidity
    /// @param to If isLong0ToLong1 then receipient of long0 positions, ekse recipient of long1 positions.
```
Change `ekse` to `else`
___
The same typo (`receipients`) occurs in all six lines below:

***ITimeswapV2Pool.sol***

[L243](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L243), [L244](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L244), [L245](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L245)

***Pool.sol***

[L179](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L179), [L180](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L180), [L181](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L181)

```solidity
    /// @dev Transfer long0 positions, long1 positions, and/or short positions to the receipients.
```
Change `receipients` to `recipients` in all cases
___
[ITimeswapV2PoolMintCallback.sol: L10](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L10)
```solidity
    /// @dev The liquidity positionss will already be minted to the receipient.
```
Change `positionss` to `positions`
___
[ConstantProduct.sol: L394](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L394)
```solidity
    /// @return sqrtDiscriminant The square root disriminant calculated.
```
Change `disriminant` to `discriminant`
___
The same typo (`retreived`) occurs in all four lines below:

***PoolFactory.sol***

[L27](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L27), [L28](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L28), [L41](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L41), [L42](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L42)

```solidity
    /// @return optionPair The retreived option pair address. Zero address if not deployed.
```
Change `retreived` to `retrieved` in all cases
___
[PoolPair.sol: L19](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolPair.sol#L19)
```solidity
    /// @dev Checks if the pool doesn not exist.
```
Change `doesn` to `does`
___
The same typo (`postion`) occurs in both lines below:

[ITimeswapV2Token.sol: L27](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L27)

[ITimeswapV2Token.sol: L32](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L32)
```solidity
    /// @dev mints TimeswapV2Token as per postion and amount
```
Change `postion` to `position` in both cases
___
The same typo (`liqudityAmount`) occurs in both lines below:

[ITimeswapV2LiquidityToken.sol: L44](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L44)

[ITimeswapV2LiquidityToken.sol: L49](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L49)
```solidity
    /// @dev mints TimeswapV2LiquidityToken as per the liqudityAmount
```
Change `liqudityAmount` to `liquidityAmount` in both cases
___
[Process.sol: L4](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Process.sol#L4)
```solidity
/// @dev Processing information required for interacting multple options in a single contract.
```
Change `multple` to `multiple`
___

The same typo (`calback`) occurs in both lines below:

[v2-option/src/structs/Param.sol: L43](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L43)

[v2-option/src/structs/Param.sol: L88](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Param.sol#L88)
```solidity
/// @notice If data length is zero, skips the calback.
```
Change `calback` to `callback` in both cases
___
The same typo (`maturies`) occurs in both lines below:

[TimeswapV2Option.sol: L31](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L31)

[TimeswapV2Option.sol: L47](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L47)
```solidity
/// @notice Holds the option of all strikes and maturies.
```
Change `maturies` to `maturities` in both cases
___



| No. | Description |
|--| ----------- | 
|4|**`pragma solidity` version should be upgraded to latest version before finalization**|
|  |The solidity version used in the in-scope contracts is either `pragma solidity ^0.8.0`, `pragma solidity ^0.8.8` or `pragma solidity =0.8.8` compared to the latest version of `0.8.17`.|
|  |There are specific upgrades that have been applied since `0.8.8`. For example, a version of at least 0.8.10 is required to have external calls skip contract existence checks if the external call has a return value, 0.8.12 is necessary for `string.concat` to be used instead of `abi.encodePacked`, and 0.8.13 or later is needed for the ability to use `using for` with a list of free functions.  In addition, only the latest version receives security fixes.|

___

| No. | Description |
|--| ----------- | 
|5-1|**Natspec completely or mostly missing for `function`**|
|  |While the majority of `functions` in the in-scope contracts are preceded by complete Natspec, it is missing or mostly missing for the `functions` below|  
|  |Note that NatSpec is required for `public` or `external` `functions` but not for `private` or `internal` ones|

___
[TimeswapV2PoolFactory.sol: L52-54](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L52-L54)
```solidity
    function numberOfPairs() external view override returns (uint256) {
        return getByIndex.length;
    }
```
___
[ITimeswapV2PoolFactory.sol: L31](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L31)
```solidity
    function getByIndex(uint256 id) external view returns (address optionPair);
```
___
[ITimeswapV2PoolFactory.sol: L33](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L33)
```solidity
    function numberOfPairs() external view returns (uint256);
```
___
[IERC1155Enumerable.sol: L14-16](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L14-L16)
```solidity
    /// @dev Returns a token ID owned by `owner` at a given `index` of its token list.
    /// Use along with {balanceOf} to enumerate all of ``owner``'s tokens.
    function tokenOfOwnerByIndex(address owner, uint256 index) external view returns (uint256);
```
Missing: `@param` and `@return` statements
___
[IERC1155Enumerable.sol: L18-20](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L18-L20)
```solidity
    /// @dev Returns a token ID at a given `index` of all the tokens stored by the contract.
    /// Use along with {totalSupply} to enumerate all tokens.
    function tokenByIndex(uint256 index) external view returns (uint256);
```
Missing: `@param` and `@return` statements
___
[TimeswapV2Option.sol: L76-77](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L76-L77)
```solidity
    function getByIndex(uint256 id) external view override returns (StrikeAndMaturity memory) {
        return listOfOptions[id];
```
___
[TimeswapV2Option.sol: L80-81](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L80-L81)
```solidity
    function numberOfOptions() external view override returns (uint256) {
        return listOfOptions.length;
```
___
[TimeswapV2Option.sol: L246](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L246)
```solidity
    function collect(TimeswapV2OptionCollectParam calldata param) external override noDelegateCall returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount, bytes memory data) {
```
___

| No. | Description |
|--| ----------- | 
|5-2|**`@param` missing for `function`**|
|  |One or more `@param` statements is missing for the following `functions`:|

___
[Pool.sol: L96-104](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L96-L104)
```solidity
    /// @dev Returns the amount of sum of long0 and long1 converted to base denomination in the pool.
    /// @dev Returns the amount of short positions in the pool.
    /// @param pool The state of the pool.
    /// @return longAmount The amount of sum of long0 and long1 converted to base denomination in the pool.
    /// @return shortAmount The amount of short in the pool.
    function totalPositions(Pool storage pool, uint256 maturity, uint96 blockTimestamp) external view returns (uint256 longAmount, uint256 shortAmount) {
        longAmount = ConstantProduct.getLong(pool.liquidity, pool.sqrtInterestRate, false);
        shortAmount = ConstantProduct.getShort(pool.liquidity, pool.sqrtInterestRate, DurationCalculation.unsafeDurationFromNowToMaturity(maturity, blockTimestamp), false);
    }
```
Missing: `@param maturity` and `@param blockTimestamp`
___
[Pool.sol: L175-183](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L175-L183)
```solidity
    /// @dev Transfer fees earned of the sender to another address.
    /// @notice Does not transfer the liquidity positions of the sender.
    /// @param pool The state of the pool.
    /// @param to The receipient of the transaction fees.
    /// @param long0Fees The amount of long0 position fees transferrred.
    /// @param long1Fees The amount of long1 position fees transferrred.
    /// @param shortFees The amount of short position fees transferrred.
    /// @param blockTimestamp The current block timestamp.
    function transferFees(Pool storage pool, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees, uint96 blockTimestamp) external {
```
Missing: `@param maturity`
___
[Pool.sol: L251-269](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L251-L269)
```solidity
    /// @dev Collects the transaction fees of the pool.
    /// @dev only liquidity provider can call this function.
    /// @dev if the owner enters an amount which is greater than the fee amount they have earned, withdraw only the amount they have.
    /// @param pool The state of the pool.
    /// @param blockTimestamp The current block timestamp.
    /// @param long0Requested The maximum amount of long0 positions wanted.
    /// @param long1Requested The maximum amount of long1 positions wanted.
    /// @param shortRequested The maximum amount of short positions wanted.
    /// @return long0Amount The amount of long0 collected.
    /// @return long1Amount The amount of long1 collected.
    /// @return shortAmount The amount of short collected.
    function collectTransactionFees(
        Pool storage pool,
        uint256 maturity,
        uint256 long0Requested,
        uint256 long1Requested,
        uint256 shortRequested,
        uint96 blockTimestamp
    ) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) {
```
Missing: `@param maturity`
___


| No. | Description |
|--| ----------- | 
|5-3|**`@return` missing for `function`**|
|  |The following `functions` have complete Natspec, with the exception of missing `@return` statements:|
___
[ITimeswapV2Pool.sol: L144-145](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L144-L145)
```solidity
    /// @dev Returns the factory address that deployed this contract.
    function poolFactory() external view returns (address);
```
___
[ITimeswapV2Pool.sol: L147-148](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L147-L148)
```solidity
    /// @dev Returns the Timeswap V2 Option of the pair.
    function optionPair() external view returns (address);
```
___
[ITimeswapV2Pool.sol: L150-151](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L150-L151)
```solidity
    /// @dev Returns the transaction fee earned by the liquidity providers.
    function transactionFee() external view returns (uint256);
```
___
[ITimeswapV2Pool.sol: L153-154](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L153-L154)
```solidity
    /// @dev Returns the protocol fee earned by the protocol.
    function protocolFee() external view returns (uint256);
```
___
[ITimeswapV2Pool.sol: L156-158](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L156-L158)
```solidity
    /// @dev Get the strike and maturity of the pool in the pool enumeration list.
    /// @param id The chosen index.
    function getByIndex(uint256 id) external view returns (StrikeAndMaturity memory);
```
___
[ITimeswapV2Pool.sol: L160-161](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L160-L161)
```solidity
    /// @dev Get the number of pools being interacted.
    function numberOfPools() external view returns (uint256);
```
___
[ITimeswapV2PoolFactory.sol: L19-20](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L19-L20)
```solidity
    /// @dev Returns the fixed transaction fee used by all created Timeswap V2 Pool contract.
    function transactionFee() external view returns (uint256);
```
___
[ITimeswapV2PoolFactory.sol: L22-23](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L22-L23)
```solidity
    /// @dev Returns the fixed protocol fee used by all created Timeswap V2 Pool contract.
    function protocolFee() external view returns (uint256);
```
___
[ITimeswapV2PoolFactory.sol: L37-41](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L37-L41)
```solidity
    /// @dev Creates a Timeswap V2 Pool based on option parameter.
    /// @dev Cannot create a duplicate Timeswap V2 Pool with the same option parameter.
    /// @param option The address of the option contract used by the pool.
    /// @param poolPair The address of the Timeswap V2 Pool contract created.
    function create(address option) external returns (address poolPair);
```
___
[ITimeswapV2PoolRebalanceCallback.sol: L8-12](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol#L8-L12)
```solidity
    /// @dev When Long0ToLong1, require the transfer of long0 position into the pool.
    /// @dev When Long1ToLong0, require the transfer of long1 position into the pool.
    /// @dev The long0 positions or long1 positions will already be minted to the receipient.
    /// @param data The bytes of data to be sent to msg.sender.
    function timeswapV2PoolRebalanceCallback(TimeswapV2PoolRebalanceCallbackParam calldata param) external returns (bytes memory data);
```
___
[ITimeswapV2PoolLeverageCallback.sol: L16-18](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L16-L18)
```solidity
    /// @dev Require the transfer of short position into the pool.
    /// @param data The bytes of data to be sent to msg.sender.
    function timeswapV2PoolLeverageCallback(TimeswapV2PoolLeverageCallbackParam calldata param) external returns (bytes memory data);
```
___
[ITimeswapV2PoolMintCallback.sol: L16-18](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L16-L18)
```solidity
    /// @dev Require the transfer of long0 position, long1 position, and short position into the pool.
    /// @param data The bytes of data to be sent to msg.sender.
    function timeswapV2PoolMintCallback(TimeswapV2PoolMintCallbackParam calldata param) external returns (bytes memory data);
```
___
[ITimeswapV2PoolDeleverageCallback.sol: L16-18](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L16-L18)
```solidity
    /// @dev Require the transfer of long0 position and long1 position into the pool.
    /// @param data The bytes of data to be sent to msg.sender.
    function timeswapV2PoolDeleverageCallback(TimeswapV2PoolDeleverageCallbackParam calldata param) external returns (bytes memory data);
```
___
[ITimeswapV2Token.sol: L9-13](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L9-L13)
```solidity
/// @title An interface for TS-V2 token system
/// @notice This interface is used to interact with TS-V2 positions
interface ITimeswapV2Token is IERC1155 {
    /// @dev Returns the factory address that deployed this contract.
    function optionFactory() external view returns (address);
```
___
[ITimeswapV2Token.sol: L15-18](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L15-L18)
```solidity
    /// @dev Returns the position Balance of the owner
    /// @param owner The owner of the token
    /// @param position type of option position (long0, long1, short)
    function positionOf(address owner, TimeswapV2TokenPosition calldata position) external view returns (uint256 amount);
```
___
[ITimeswapV2Token.sol: L32-34](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L32-L34)
```solidity
    /// @dev burns TimeswapV2Token as per postion and amount
    /// @param param The TimeswapV2TokenBurnParam
    function burn(TimeswapV2TokenBurnParam calldata param) external returns (bytes memory data);
```
___
[ITimeswapV2LiquidityToken.sol: L23-26](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L23-L26)
```solidity
    /// @dev Returns the position Balance of the owner
    /// @param owner The owner of the token
    /// @param position The liquidity position
    function positionOf(address owner, TimeswapV2LiquidityTokenPosition calldata position) external view returns (uint256 amount);
```
___
[ITimeswapV2LiquidityToken.sol: L54-59](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L54-L59)
```solidity
    /// @dev collects fees as per the fees desired
    /// @param param The TimeswapV2LiquidityTokenBurnParam
    /// @return long0Fees Fees for long0
    /// @return long1Fees Fees for long1
    /// @return shortFees Fees for short
    function collect(TimeswapV2LiquidityTokenCollectParam calldata param) external returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees, bytes memory data);
```
Missing: `@return data`
___
[IERC1155Enumerable.sol: L8-12](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L8-L12)
```solidity
/// @title ERC-1155 Token Standard, optional enumeration extension
/// @dev See https://eips.ethereum.org/EIPS/eip-721
interface IERC1155Enumerable is IERC1155 {
    /// @dev Returns the total amount of tokens stored by the contract.
    function totalSupply() external view returns (uint256);
```
___
[ITimeswapV2Option.sol: L109-110](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L109-L110)
```solidity
    /// @dev Returns the factory address that deployed this contract.
    function optionFactory() external view returns (address);
```
___
[ITimeswapV2Option.sol: L112-113](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L112-L113)
```solidity
    /// @dev Returns the first ERC20 token address of the pair.
    function token0() external view returns (address);
```
___
[ITimeswapV2Option.sol: L115-116](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L115-L116)
```solidity
    /// @dev Returns the second ERC20 token address of the pair.
    function token1() external view returns (address);
```
___
[ITimeswapV2Option.sol: L118-120](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L118-L120)
```solidity
    /// @dev Get the strike and maturity of the option in the option enumeration list.
    /// @param id The chosen index.
    function getByIndex(uint256 id) external view returns (StrikeAndMaturity memory);
```
___
[ITimeswapV2Option.sol: L122-123](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L122-L123)
```solidity
    /// @dev Number of options being interacted.
    function numberOfOptions() external view returns (uint256);
```
___
[ITimeswapV2Option.sol: L161-169](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L161-L169)
```solidity
    /// @dev Burn short position.
    /// Withdraw token0, when long token0 is burnt.
    /// Withdraw token1, when long token1 is burnt.
    /// @dev Can only be called before the maturity of the pool.
    /// @param param The parameters for the burn function.
    /// @return token0AndLong0Amount The amount of token0 withdrawn and long0 burnt.
    /// @return token1AndLong1Amount The amount of token1 withdrawn and long1 burnt.
    /// @return shortAmount The amount of short burnt.
    function burn(TimeswapV2OptionBurnParam calldata param) external returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data);
```
Missing: `@return data`
___
[ITimeswapV2Option.sol: L184-190](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L184-L190)
```solidity
    /// @dev Burn short position, withdraw token0 and token1.
    /// @dev Can only be called after the maturity of the pool.
    /// @param param The parameters for the collect function.
    /// @return token0Amount The amount of token0 withdrawn.
    /// @return token1Amount The amount of token1 withdrawn.
    /// @return shortAmount The amount of short burnt.
    function collect(TimeswapV2OptionCollectParam calldata param) external returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount, bytes memory data);
```
Missing: `@return data`
___
[ITimeswapV2OptionDeployer.sol: L11-16](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol#L11-L16)
```solidity
    /// @notice Get the parameters to be used in constructing the pair, set transiently during pair creation.
    /// @dev Called by the pair constructor to fetch the parameters of the pair.
    /// @return optionFactory The factory address.
    /// @param token0 The first ERC20 token address of the pair.
    /// @param token1 The second ERC20 token address of the pair.
    function parameter() external view returns (address optionFactory, address token0, address token1);
```
Notice: `@param` statements should be `@return` statements
___
[ITimeswapV2OptionFactory.sol: L26-28](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L26-L28)
```solidity
    /// @dev Get the address of the option pair in the option pair enumeration list.
    /// @param id The chosen index.
    function getByIndex(uint256 id) external view returns (address optionPair);
```
___
[ITimeswapV2OptionFactory.sol: L30-31](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L30-L31)
```solidity
    /// @dev The number of option pairs deployed.
    function numberOfPairs() external view returns (uint256);
```
___
[ITimeswapV2OptionFactory.sol: L35-41](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L35-L41)
```solidity
    /// @dev Creates a Timeswap V2 Option based on pair parameters.
    /// @dev Cannot create a duplicate Timeswap V2 Option with the same pair parameters.
    /// @notice The token0 address must be smaller than token1 address.
    /// @param token0 The first ERC20 token address of the pair.
    /// @param token1 The second ERC20 token address of the pair.
    /// @param optionPair The address of the Timeswap V2 Option contract created.
    function create(address token0, address token1) external returns (address optionPair);
```
Notice: `@param optionPair` should be `@return optionPair`
___


| No. | Description |
|--| ----------- | 
|5-4|**Natspec is missing for `event`**|
|  |While the majority of `events` in the in-scope contracts are preceded by complete Natspec, it is missing for the following `event`:|

___
[ITimeswapV2LiquidityToken.sol: L13](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L13)
```solidity
    event TransferFees(address from, address to, TimeswapV2LiquidityTokenPosition position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees);
```
___
___
