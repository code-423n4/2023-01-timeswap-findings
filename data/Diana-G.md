
## G-01 x += y costs more gas than x = x + y for state variables (same applies for x -= y)

Using the addition operator instead of plus-equals saves **[113 gas](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)**.

_There are 73 instances of this issue_

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-library/src/FullMath.sol

```
File: main/packages/v2-library/src/FullMath.sol

163: productA1 += (quotient1 * divisor);
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol

```
File: main/packages/v2-option/src/TimeswapV2Option.sol

190: option.long0[msg.sender] -= token0AndLong0Amount;
191: option.long1[msg.sender] -= token1AndLong1Amount;
192: option.short[msg.sender] -= shortAmount;
237: if (param.isLong0ToLong1) option.long0[msg.sender] -= token0AndLong0Amount;
238: else option.long1[msg.sender] -= token1AndLong1Amount;
277: option.short[msg.sender] -= shortAmount;
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-option/src/structs/Option.sol

```
File: main/packages/v2-option/src/structs/Option.sol

62: option.long0[msg.sender] -= amount;
63: option.long0[to] += amount;
65: option.long1[to] += amount;
66: option.long1[msg.sender] -= amount;
68: option.short[msg.sender] -= amount;
69: option.short[to] += amount;
103: option.totalLong0 += token0AndLong0Amount;
104: option.long0[long0To] += token0AndLong0Amount;
108: option.totalLong1 += token1AndLong1Amount;
109: option.long1[long1To] += token1AndLong1Amount;
112: option.short[shortTo] += shortAmount;
141: option.totalLong0 -= token0AndLong0Amount;
142: option.totalLong1 -= token1AndLong1Amount;
173: option.totalLong0 -= token0AndLong0Amount;
174: option.totalLong1 += token1AndLong1Amount;
175: option.long1[longTo] += token1AndLong1Amount;
177: option.totalLong1 -= token1AndLong1Amount;
178: option.totalLong0 += token0AndLong0Amount;
179: option.long0[longTo] += token0AndLong0Amount;
208: option.totalLong0 -= token0Amount;
209: option.totalLong1 -= token1Amount;
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol

```
File: main/packages/v2-pool/src/libraries/ConstantProduct.sol

149: if (isAdd) longAmount -= fees;
150: else shortAmount -= fees;
180: shortAmount -= fees;
212: longAmount -= fees;
244: amount -= fees;
295: denominator2 += longAmount;
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol

```
File: main/packages/v2-pool/src/structs/LiquidityPosition.sol

55: liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth);
56: liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth);
57: liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth);
66: liquidityPosition.liquidity += liquidityAmount;
70: liquidityPosition.long0Fees += long0Fees;
71: liquidityPosition.long1Fees += long1Fees;
72: liquidityPosition.shortFees += shortFees;
76: liquidityPosition.liquidity -= liquidityAmount;
80: liquidityPosition.long0Fees -= long0Fees;
81: liquidityPosition.long1Fees -= long1Fees;
82: liquidityPosition.shortFees -= shortFees;
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol

```
File: main/packages/v2-pool/src/structs/Pool.sol

361: if (long0Amount != 0) pool.long0Balance += long0Amount;
362: if (long1Amount != 0) pool.long1Balance += long1Amount;
365: pool.liquidity += liquidityAmount;
452: if (long0Amount != 0) pool.long0Balance -= long0Amount;
453: if (long1Amount != 0) pool.long1Balance -= long1Amount;
455: pool.liquidity -= liquidityAmount;
537: if (long0Amount != 0) pool.long0Balance += long0Amount;
538: if (long1Amount != 0) pool.long1Balance += long1Amount;
636: pool.long0Balance -= (long0Amount + long0Fees);
650: pool.long1Balance -= (long1Amount + long1Fees);
677: pool.long1Balance -= (long1Amount + longFees);
689: pool.long1Balance -= (long1Amount + longFees);
695: pool.long0Balance += long0Amount;
710: pool.long0Balance -= (long0Amount + longFees);
719: pool.long0Balance -= (long0Amount + longFees);
722: pool.long1Balance += long1Amount;
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol

```
File: main/packages/v2-token/src/base/ERC1155Enumerable.sol

61: _idTotalSupply[id] += amount;
95: _idTotalSupply[id] -= amount;
117: _currentIndex[to] += 1;
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol

```
File: main/packages/v2-token/src/structs/FeesPosition.sol

31: feesPosition.long0Fees += FeeCalculation.getFees(liquidity, feesPosition.long0FeeGrowth, long0FeeGrowth);
32: feesPosition.long1Fees += FeeCalculation.getFees(liquidity, feesPosition.long1FeeGrowth, long1FeeGrowth);
33: feesPosition.shortFees += FeeCalculation.getFees(liquidity, feesPosition.shortFeeGrowth, shortFeeGrowth);
53: feesPosition.long0Fees += long0Fees;
54: feesPosition.long1Fees += long1Fees;
55: feesPosition.shortFees += shortFees;
59: feesPosition.long0Fees -= long0Fees;
60: feesPosition.long1Fees -= long1Fees;
61: feesPosition.shortFees -= shortFees;
```

-----

## G-02 Duplicated require() or revert() checks should be refactored to a modifier or function

_There are 12 instances of this issue_

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol

```
File: main/packages/v2-option/src/enums/Transaction.sol

37: if (uint256(transaction) >= 2) revert InvalidTransaction();
43: if (uint256(transaction) >= 2) revert InvalidTransaction();
49: if (uint256(transaction) >= 2) revert InvalidTransaction();
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol

```
File: main/packages/v2-pool/src/TimeswapV2Pool.sol

289: if (isQuote) revert Quote();
355: if (isQuote) revert Quote();
407: if (isQuote) revert Quote();
457: if (isQuote) revert Quote();
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol

```
File: main/packages/v2-pool/src/enums/Transaction.sol

53: if (uint256(transaction) >= 4) revert InvalidTransaction();
58: if (uint256(transaction) >= 4) revert InvalidTransaction();
63: if (uint256(transaction) >= 4) revert InvalidTransaction();
68: if (uint256(transaction) >= 4) revert InvalidTransaction();
73: if (uint256(transaction) >= 2) revert InvalidTransaction();
```

-------

## G-03 Not using the named return variables when a function returns, wastes deployment gas

_There is 15 instances of this issue_

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

-------

## G-04 USAGE OF UINTS OR INTS SMALLER THAN 32 BYPTES INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Each operation involving a `uint8` costs an extra [**22-28 gas**](https://gist.github.com/IllIllI000/9388d20c70f9a4632eb3ca7836f54977) (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

_There are 19 instances of this issue:_

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-library/src/Error.sol

```
File: main/packages/v2-library/src/Error.sol

17: error AlreadyHaveLiquidity(uint160 liquidity);
50: error AlreadyMatured(uint256 maturity, uint96 blockTimestamp);
55: error StillActive(uint256 maturity, uint96 blockTimestamp);
83: function alreadyHaveLiquidity(uint160 liquidity) internal pure {
113: function stillActive(uint256 maturity, uint96 blockTimestamp) internal pure {
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-library/src/SafeCast.sol

```
File: main/packages/v2-library/src/SafeCast.sol

18: function toUint16(uint256 value) internal pure returns (uint16 result) {
27: function toUint96(uint256 value) internal pure returns (uint96 result)
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol

```
File: main/packages/v2-pool/src/structs/Param.sol

103: function check(TimeswapV2OptionMintParam memory param, uint96 blockTimestamp) internal pure {
117: function check(TimeswapV2OptionBurnParam memory param, uint96 blockTimestamp) internal pure {
130: function check(TimeswapV2OptionSwapParam memory param, uint96 blockTimestamp) internal pure {
143: function check(TimeswapV2OptionCollectParam memory param, uint96 blockTimestamp) internal pure {
```

https://github.com/code-423n4/2022-10-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol

```
File: main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol

16: event TransferLiquidity(uint256 indexed strike, uint256 indexed maturity, address indexed from, address to, uint160 liquidityAmount);
81: event Mint(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);
101: uint160 liquidityAmount,
167: function totalLiquidity(uint256 strike, uint256 maturity) external view returns (uint160 liquidityAmount);
174: function sqrtInterestRate(uint256 strike, uint256 maturity) external view returns (uint160 rate);
181: function liquidityOf(uint256 strike, uint256 maturity, address owner) external view returns (uint160 liquidityAmount);
236: function transferLiquidity(uint256 strike, uint256 maturity, address to, uint160 liquidityAmount) external
252: function initialize(uint256 strike, uint256 maturity, uint160 rate) external;
```

----------

