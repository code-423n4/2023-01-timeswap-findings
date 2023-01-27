# Summary

## Gas Optimizations

|        | Issue                                                                                                                      | Instances |
| ------ | :------------------------------------------------------------------------------------------------------------------------- | :-------: |
| GAS-1  | Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`, where appropriate |     4     |
| GAS-2  | Short-circuiting                                                                                                           |     1     |
| GAS-3  | Use bitmaps to save gas                                                                                                    |     1     |
| GAS-4  | `<x> += <y>`/`<x> -= <y>` costs more gas than `<x> = <x> + <y>`/`<x> = <x> - <y>` for state variables                      |    73     |
| GAS-5  | Internal functions only called once can be inlined to save gas                                                             |     8     |
| GAS-6  | Optimize names to save gas                                                                                                 |     8     |
| GAS-7  | Using unchecked blocks to save gas                                                                                         |     1     |
| GAS-8  | Using storage instead of memory for structs/arrays saves gas                                                               |     1     |
| GAS-9  | Usage of uint/int smaller than 32 bytes (256 bits) incurs overhead                                                         |    62     |
| GAS-10 | Functions not used internally could be marked external                                                                     |     1     |
| GAS-11 | Setting the constructor to payable                                                                                         |     5     |

Total: 165 contexts over 11 issues

### [GAS-1] Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

##### **Proof Of Concept**

- `options` and `hasInteracted` in `transferPosition` function,

  [TimeswapV2Option.sol#L48](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L48)

  [TimeswapV2Option.sol#L53](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L53)

- `_timeswapV2LiquidityTokenPositions` and `_timeswapV2LiquidityTokenPositionIds` in `mint` function,

  [TimeswapV2LiquidityToken.sol#L43](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L43)

  [TimeswapV2LiquidityToken.sol#L45](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L45)

- `_timeswapV2LiquidityTokenPositions` and `_feesPositions` in `transferFeesFrom` function,

  [TimeswapV2LiquidityToken.sol#L43](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L43)

  [TimeswapV2LiquidityToken.sol#L47](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L47)

- `_timeswapV2TokenPositions` and `_timeswapV2TokenPositionIds` in `mint` function,

  [TimeswapV2Token.sol#L38](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L38)

  [TimeswapV2Token.sol#L39](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L39)

- `_currentIndex`, `_ownedTokens` and` _ownedTokensIndex` in` _addTokenToOwnerEnumeration` function,

  [ERC1155Enumerable.sol#L15](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L15)

  [ERC1155Enumerable.sol#L17](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L17)

  [ERC1155Enumerable.sol#L20](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L20)

- `_currentIndex` and `_ownedTokens` in `_removeTokenFromOwnerEnumeration` function

  [ERC1155Enumerable.sol#L15](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L15)

  [ERC1155Enumerable.sol#L17](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L17)

All of them are being used in the same functions mostly consider making them a structs instead.

### [GAS-2] Short-circuiting

Short-circuiting is a strategy we can make use of when an operation makes use of either || or &&. This pattern works by ordering the lower-cost operation first so that the higher-cost operation may be skipped (short-circuited) if the first operation evaluates to true.

##### **Proof Of Concept**

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

298           if (product.div(longAmount, false) != rate || product >= numerator) revert NotEnoughLiquidityToBorrow();

```

[ConstantProduct.sol#L298](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L298)

Consider changing order from
`if (product.div(longAmount, false) != rate || product >= numerator) revert NotEnoughLiquidityToBorrow();`
to
`if (product >= numerator || product.div(longAmount, false) != rate) revert NotEnoughLiquidityToBorrow();`

### [GAS-3] Use bitmaps to save gas

During calls to `hasInteracted` the executed mapping is updated, setting the bool flag to true, but because the way the EVM works it allocates an entire storage slot (256 bits) every time a new flag is set. SSTORE opcode cost up to 20000 gas for uninitialized slots like these. Consider using bitmaps instead, this enables you to convert the `mapping(uint256 => bool)` to a `mapping(uint256 => uint256)` and pay the cost of slot initialization just once every 256 nonces added, so the high gas costs are amortized over many calls.

##### **Proof Of Concept**

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

53    mapping(uint256 => mapping(uint256 => bool)) private hasInteracted;

```

[TimeswapV2Option.sol#L53](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L53)

Consider changing `mapping(uint256 => mapping(uint256 => bool)) private hasInteracted;` to `mapping(uint256 => mapping(uint256 => uint256)) private hasInteracted;`.

### [GAS-4] `<x> += <y>`/`<x> -= <y>` costs more gas than `<x> = <x> + <y>`/`<x> = <x> - <y>` for state variables

Using compound assignment operators for state variables (like `State += X` or S`tate -= X` …) it’s more expensive than using operator assignment (like `State = State + X` or `State = State - X` …).

##### **Proof Of Concept**

```solidity
File: packages/v2-library/src/FullMath.sol

163    productA1 += (quotient1 * divisor);

```

[FullMath.sol#L163](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L163)

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

190:         option.long0[msg.sender] -= token0AndLong0Amount;

191:         option.long1[msg.sender] -= token1AndLong1Amount;

192:         option.short[msg.sender] -= shortAmount;

237:         if (param.isLong0ToLong1) option.long0[msg.sender] -= token0AndLong0Amount;

238:         else option.long1[msg.sender] -= token1AndLong1Amount;

277:         option.short[msg.sender] -= shortAmount;

```

[TimeswapV2Option.sol#L190](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L190)

```solidity
File: packages/v2-option/src/structs/Option.sol
62:             option.long0[msg.sender] -= amount;

63:             option.long0[to] += amount;

65:             option.long1[to] += amount;

66:             option.long1[msg.sender] -= amount;

68:             option.short[msg.sender] -= amount;

69:             option.short[to] += amount;

103:             option.totalLong0 += token0AndLong0Amount;

104:             option.long0[long0To] += token0AndLong0Amount;

108:             option.totalLong1 += token1AndLong1Amount;

109:             option.long1[long1To] += token1AndLong1Amount;

112:         option.short[shortTo] += shortAmount;

141:         option.totalLong0 -= token0AndLong0Amount;

142:         option.totalLong1 -= token1AndLong1Amount;

173:             option.totalLong0 -= token0AndLong0Amount;

174:             option.totalLong1 += token1AndLong1Amount;

175:             option.long1[longTo] += token1AndLong1Amount;

177:             option.totalLong1 -= token1AndLong1Amount;

178:             option.totalLong0 += token0AndLong0Amount;

179:             option.long0[longTo] += token0AndLong0Amount;

208:         option.totalLong0 -= token0Amount;

209:         option.totalLong1 -= token1Amount;
```

[Option.sol#L62](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L62)

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol
149:         if (isAdd) longAmount -= fees;

150:         else shortAmount -= fees;

180:             shortAmount -= fees;

212:             longAmount -= fees;

244:         amount -= fees;

295:             denominator2 += longAmount;
```

[ConstantProduct.sol#L149](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L149)

```solidity
File: packages/v2-pool/src/structs/LiquidityPosition.sol

55:             liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth);

56:             liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth);

57:             liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth);

66:         liquidityPosition.liquidity += liquidityAmount;

70:         liquidityPosition.long0Fees += long0Fees;

71:         liquidityPosition.long1Fees += long1Fees;

72:         liquidityPosition.shortFees += shortFees;

76:         liquidityPosition.liquidity -= liquidityAmount;

80:         liquidityPosition.long0Fees -= long0Fees;

81:         liquidityPosition.long1Fees -= long1Fees;

82:         liquidityPosition.shortFees -= shortFees;
```

[LiquidityPosition.sol#L55](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L55)

```solidity
File: packages/v2-pool/src/structs/Pool.sol
361:         if (long0Amount != 0) pool.long0Balance += long0Amount;

362:         if (long1Amount != 0) pool.long1Balance += long1Amount;

365:         pool.liquidity += liquidityAmount;

452:         if (long0Amount != 0) pool.long0Balance -= long0Amount;

453:         if (long1Amount != 0) pool.long1Balance -= long1Amount;

455:         pool.liquidity -= liquidityAmount;

537:         if (long0Amount != 0) pool.long0Balance += long0Amount;

538:         if (long1Amount != 0) pool.long1Balance += long1Amount;

636:                 pool.long0Balance -= (long0Amount + long0Fees);

650:                 pool.long1Balance -= (long1Amount + long1Fees);

677:                 pool.long1Balance -= (long1Amount + longFees);

689:                     pool.long1Balance -= (long1Amount + longFees);

695:             pool.long0Balance += long0Amount;

710:                     pool.long0Balance -= (long0Amount + longFees);

719:                 pool.long0Balance -= (long0Amount + longFees);

722:             pool.long1Balance += long1Amount;
```

[Pool.sol#L361](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L361)

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

61:             _idTotalSupply[id] += amount;

95:             _idTotalSupply[id] -= amount;

117:         _currentIndex[to] += 1;
```

[ERC1155Enumerable.sol#L61](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L61)

```solidity
File: packages/v2-token/src/structs/FeesPosition.sol
31:             feesPosition.long0Fees += FeeCalculation.getFees(liquidity, feesPosition.long0FeeGrowth, long0FeeGrowth);

32:             feesPosition.long1Fees += FeeCalculation.getFees(liquidity, feesPosition.long1FeeGrowth, long1FeeGrowth);

33:             feesPosition.shortFees += FeeCalculation.getFees(liquidity, feesPosition.shortFeeGrowth, shortFeeGrowth);

53:         feesPosition.long0Fees += long0Fees;

54:         feesPosition.long1Fees += long1Fees;

55:         feesPosition.shortFees += shortFees;

59:         feesPosition.long0Fees -= long0Fees;

60:         feesPosition.long1Fees -= long1Fees;

61:         feesPosition.shortFees -= shortFees;
```

[FeesPosition.sol#L31](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L31)

### [GAS-5] Internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

##### **Proof Of Concept**

```solidity
File: packages/v2-pool/src/libraries/FeeCalculation.sol

61:     function getFeesRemoval(uint256 amount, uint256 fee) internal pure returns (uint256) {

75:     function getFeeGrowth(uint256 feeAmount, uint160 liquidity) internal pure returns (uint256) {

```

[FeeCalculation.sol#L61](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L61)

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

58:     function _addTokenEnumeration(address from, address to, uint256 id, uint256 amount) internal {

70:     function _additionalConditionAddTokenToAllTokensEnumeration(uint256) internal virtual returns (bool) {

75:     function _additionalConditionAddTokenToOwnerEnumeration(address, uint256) internal virtual returns (bool) {

92:     function _removeTokenEnumeration(address from, address to, uint256 id, uint256 amount) internal {

104:     function _additionalConditionRemoveTokenFromAllTokensEnumeration(uint256) internal virtual returns (bool) {

109:     function _additionalConditionRemoveTokenFromOwnerEnumeration(address, uint256) internal virtual returns (bool) {

```

[ERC1155Enumerable.sol#L61](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L61)

### [GAS-6] Optimize names to save gas

`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. In this report are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92).

##### **Proof Of Concept**

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

32: contract TimeswapV2Option is ITimeswapV2Option, NoDelegateCall {

```

[TimeswapV2Option.sol#L32](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L32)

```solidity
File: packages/v2-option/src/TimeswapV2OptionFactory.sol

17: contract TimeswapV2OptionFactory is ITimeswapV2OptionFactory, TimeswapV2OptionDeployer {

```

[TimeswapV2OptionFactory.sol#L17](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L17)

```solidity
File: packages/v2-option/src/interfaces/ITimeswapV2Option.sol

11: interface ITimeswapV2Option {

```

[ITimeswapV2Option.sol#L11](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L11)

```solidity
File: packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol

6: interface ITimeswapV2OptionFactory {

```

[ITimeswapV2OptionFactory.sol#L6](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L6)

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

36: contract TimeswapV2Pool is ITimeswapV2Pool, NoDelegateCall {

```

[TimeswapV2Pool.sol#L36](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L36)

```solidity
File: packages/v2-pool/src/TimeswapV2PoolFactory.sol

16: contract TimeswapV2PoolFactory is ITimeswapV2PoolFactory, TimeswapV2PoolDeployer, OwnableTwoSteps {

```

[TimeswapV2PoolFactory.sol#L16](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L16)

```solidity
File: packages/v2-pool/src/base/OwnableTwoSteps.sol

10: contract OwnableTwoSteps is IOwnableTwoSteps {

```

[OwnableTwoSteps.sol#L10](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L10)

```solidity
File: packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol

4: interface IOwnableTwoSteps {

```

[IOwnableTwoSteps.sol#L4](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol#L4)

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol

9: interface ITimeswapV2Pool {

```

[ITimeswapV2Pool.sol#L9](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L9)

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol

8: interface ITimeswapV2PoolFactory is IOwnableTwoSteps {

```

[ITimeswapV2PoolFactory.sol#L8)](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L8)

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol

7: interface ITimeswapV2PoolDeleverageCallback {

```

[ITimeswapV2PoolDeleverageCallback.sol#L7](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L7)

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol

7: interface ITimeswapV2PoolLeverageCallback {

```

[ITimeswapV2PoolLeverageCallback.sol#L7](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L7)

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol

7: interface ITimeswapV2PoolMintCallback {

```

[ITimeswapV2PoolMintCallback.sol#L7](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L7)

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

27: contract TimeswapV2LiquidityToken is ITimeswapV2LiquidityToken, ERC1155Enumerable {

```

[TimeswapV2LiquidityToken.sol#L27](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L27)

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

29: contract TimeswapV2Token is ITimeswapV2Token, ERC1155Enumerable {

```

[TimeswapV2Token.sol#L29](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L29)

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

13: abstract contract ERC1155Enumerable is IERC1155Enumerable, ERC1155 {

```

[ERC1155Enumerable.sol#L13](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L13)

```solidity
File: packages/v2-token/src/interfaces/IERC1155Enumerable.sol

10: interface IERC1155Enumerable is IERC1155 {
```

[IERC1155Enumerable.sol#L10](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L10)

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol

10: interface ITimeswapV2LiquidityToken is IERC1155Enumerable {
```

[ITimeswapV2LiquidityToken.sol#L10](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L10)

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2Token.sol

11: interface ITimeswapV2Token is IERC1155 {
```

[ITimeswapV2Token.sol#L11](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L11)

### [GAS-7] Using unchecked blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block [see resource](https://github.com/ethereum/solidity/issues/10695)

##### **Proof Of Concept**

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

319:         else if (rate > deltaRate) newRate = rate - deltaRate;

```

[ConstantProduct.sol#L319](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L319)

The operation `newRate = rate - deltaRate;` cannot underflow due to the check on Line 319 that ensures that `rate` is greater than `deltaRate` before perfoming the arithmetic operation.

### [GAS-8] Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read.

The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct.

##### **Proof Of Concept**

```solidity
File: packages/v2-token/src/structs/FeesPosition.sol

18:         FeesPosition memory feesPosition,

```

[FeesPosition.sol#L18](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L18)

### [GAS-9] Usage of `uint`/`int` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html

Use a larger size then downcast where needed.

##### **Proof Of Concept**

```solidity
File: packages/v2-library/src/Error.sol

50:     error AlreadyMatured(uint256 maturity, uint96 blockTimestamp);

55:     error StillActive(uint256 maturity, uint96 blockTimestamp);

106:     function alreadyMatured(uint256 maturity, uint96 blockTimestamp) internal pure {

113:     function stillActive(uint256 maturity, uint96 blockTimestamp) internal pure {

```

```solidity
File: packages/v2-library/src/Math.sol

59:     function shr(uint256 dividend, uint8 divisorBit, bool roundUp) internal pure returns (uint256 quotient) {

```

```solidity
File: packages/v2-option/src/structs/Param.sol

103:     function check(TimeswapV2OptionMintParam memory param, uint96 blockTimestamp) internal pure {

117:     function check(TimeswapV2OptionBurnParam memory param, uint96 blockTimestamp) internal pure {

130:     function check(TimeswapV2OptionSwapParam memory param, uint96 blockTimestamp) internal pure {

143:     function check(TimeswapV2OptionCollectParam memory param, uint96 blockTimestamp) internal pure {

```

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

247:         uint96 durationForward

255:         uint96 durationForward

312:         uint96 durationForward

320:         uint96 durationForward

372:         uint96 durationForward

380:         uint96 durationForward

426:     function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

433:         uint96 durationForward

```

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol

294:         uint96 durationForward

323:         uint96 durationForward

344:     function deleverage(TimeswapV2PoolDeleverageParam calldata param, uint96 durationForward) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);

364:     function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);

```

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

38:     function getShort(uint160 liquidity, uint160 rate, uint96 duration, bool roundUp) internal pure returns (uint256) {

51:     function calculateGivenLiquidityDelta(uint160 rate, uint160 deltaLiquidity, uint96 duration, bool isAdd) internal pure returns (uint256 longAmount, uint256 shortAmount) {

66:     function calculateGivenLiquidityLong(uint160 rate, uint256 longAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 shortAmount) {

81:     function calculateGivenLiquidityShort(uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 longAmount) {

101:         uint96 duration,

138:         uint96 duration,

168:         uint96 duration,

199:         uint96 duration,

234:         uint96 duration,

268:     function getLiquidityGivenShort(uint160 rate, uint256 shortAmount, uint96 duration, bool roundUp) private pure returns (uint160) {

313:     function getNewSqrtInterestRateGivenShort(uint160 liquidity, uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) private pure returns (uint160 newRate, uint160 deltaRate) {

342:     function getShortFromSqrtInterestRate(uint160 liquidity, uint160 deltaRate, uint96 duration, bool roundUp) private pure returns (uint256) {

356:     function getShortOrLongFromGivenSum(uint160 liquidity, uint160 rate, uint256 sumAmount, uint96 duration, uint256 transactionFee, bool isShort) private pure returns (uint256 amount) {

373:     function calculateNegativeB(uint160 liquidity, uint160 rate, uint256 sumAmount, uint96 duration, uint256 transactionFee, bool isShort) private pure returns (uint256 negativeB) {

399:         uint96 duration,

```

```solidity
File: packages/v2-pool/src/libraries/DurationCalculation.sol

18:     function unsafeDurationFromLastTimestampToNow(uint96 lastTimestamp, uint96 blockTimestamp) internal pure returns (uint96 duration) {

26:     function unsafeDurationFromNowToMaturity(uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {

34:     function unsafeDurationFromLastTimestampToMaturity(uint96 lastTimestamp, uint256 maturity) internal pure returns (uint96 duration) {

43:     function unsafeDurationFromLastTimestampToNowOrMaturity(uint96 lastTimestamp, uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {

```

```solidity
File: packages/v2-pool/src/libraries/ReentrancyGuard.sol

12:     uint96 internal constant NOT_INTERACTED = 0;

15:     uint96 internal constant NOT_ENTERED = 1;

18:     uint96 internal constant ENTERED = 2;

23:     function check(uint96 reentrancyGuard) internal pure {

```

```solidity
File: packages/v2-pool/src/structs/Param.sol

142:     function check(TimeswapV2PoolMintParam memory param, uint96 blockTimestamp) internal pure {

153:     function check(TimeswapV2PoolBurnParam memory param, uint96 blockTimestamp) internal pure {

165:     function check(TimeswapV2PoolDeleverageParam memory param, uint96 blockTimestamp) internal pure {

176:     function check(TimeswapV2PoolLeverageParam memory param, uint96 blockTimestamp) internal pure {

188:     function check(TimeswapV2PoolRebalanceParam memory param, uint96 blockTimestamp) internal pure {

```

```solidity
File: packages/v2-pool/src/structs/Pool.sol

43:     uint96 lastTimestamp;

101:     function totalPositions(Pool storage pool, uint256 maturity, uint96 blockTimestamp) external view returns (uint256 longAmount, uint256 shortAmount) {

118:         uint96 duration,

119:         uint96 blockTimestamp

128:     function updateDurationWeightBeforeMaturity(Pool storage pool, uint96 blockTimestamp) private {

142:     function updateDurationWeightAfterMaturity(Pool storage pool, uint256 maturity, uint96 blockTimestamp) private {

158:     function transferLiquidity(Pool storage pool, address to, uint160 liquidityAmount, uint96 blockTimestamp) external {

183:     function transferFees(Pool storage pool, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees, uint96 blockTimestamp) external {

268:         uint96 blockTimestamp

297:         uint96 blockTimestamp

383:         uint96 blockTimestamp

474:         uint96 blockTimestamp

557:         uint96 blockTimestamp

```

### [GAS-10] Functions not used internally could be marked external

Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents’ functions and change the visibility from external to public and can save gas by doing so.

##### **Proof Of Concept**

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

36:     function totalSupply() public view override returns (uint256) {

```

[ERC1155Enumerable.sol#L36](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L36)

### [GAS-11] Setting the constructor to payable

Saves ~13 gas per instance

##### **Proof Of Concept**

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

65:     constructor() NoDelegateCall() {
66:          (optionFactory, token0, token1) = ITimeswapV2OptionDeployer(msg.sender).parameter();
67:     }

```

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

77:    constructor() NoDelegateCall() {
78:        (poolFactory, optionPair, transactionFee, protocolFee) = ITimeswapV2PoolDeployer(msg.sender).parameter();
79:    }

```

```solidity
File: packages/v2-pool/src/TimeswapV2PoolFactory.sol

37:     constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) {

```

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

36:     constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {

```

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

41:     constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {

```