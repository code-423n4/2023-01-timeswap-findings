# GAS OPTIMIZATION REPORT

---

### Summary of optimizations


|              | Issue                                                                                                         | Instances |
| -------------- | --------------------------------------------------------------------------------------------------------------- | :---------: |
| [G-1](#G1)   | Use calldata instead of memory.                                                                               |     2     |
| [G-2](#G2)   | Use shift right/left instead of division/multiplication if possible.                                          |     6     |
| [G-3](#G3)   | Use assembly to check for address(0)                                                                          |    13    |
| [G-4](#G4)   | Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate. |     3     |
| [G-5](#G7)   | Usage of`uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead.                                      |    102    |
| [G-6](#G10) | `abi.encode()` is less efficient than `abi.encodePacked()`                                                    |     7     |
| [G-7](#G11) | Internal functions only called once can be inlined to save gas.                                               |    2    |
| [G-8](#G12) | `>=` costs less gas than `>`.                                                                                 |    36    |

*Total: 168 Instances over 8 issues.*

*Note: The issues already identified in the c4dit report that are shown above refer to different instances.*

---

### Gas Optimizations

### <a id=G1>[G-1]</a> Use calldata instead of memory.

##### Description

Use calldata instead of memory for function parameters saves gas if the function argument is only read.

##### Recommendation

Use calldata instead of memory.

##### *Instances (2):*

File: [2023-01-timeswap/packages/v2-library/src/BytesLib.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/BytesLib.sol#L12 )

```solidity
12: function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {
```

File: [2023-01-timeswap/packages/v2-library/src/CatchError.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/CatchError.sol#L12 )

```solidity
12: function catchError(bytes memory reason, bytes4 selector) internal pure returns (bytes memory) {
```

### <a id=G2>[G-2]</a> Use shift right/left instead of division/multiplication if possible.

##### Description

A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left.
While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas.
urthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

##### Recommendation

You should change multiplication and division with powers of two to bit shift.

##### *Instances (6):*

File: [2023-01-timeswap/packages/v2-library/src/FullMath.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/FullMath.sol#L187 )

```solidity
241: inv *= 2 - divisor * inv; // inverse mod 2**8
242: inv *= 2 - divisor * inv; // inverse mod 2**16
243: inv *= 2 - divisor * inv; // inverse mod 2**32
244: inv *= 2 - divisor * inv; // inverse mod 2**64
245: inv *= 2 - divisor * inv; // inverse mod 2**128
246: inv *= 2 - divisor * inv; // inverse mod 2**256
```

### <a id=G3>[G-3]</a> Use assembly to check for address(0)

##### Description

You can save about 6 gas per instance if using assembly to check for address(0)

##### Recommendation

Use assembly.

##### *Instances (13):*

File: [2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L76 )

```solidity
76: if (from == address(0)) Error.zeroAddress();
77: if (to == address(0)) Error.zeroAddress();
249: if (from != address(0)) _feesPositions[id][from].update(uint160(balanceOf(from, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
251: if (to != address(0)) _feesPositions[id][to].update(uint160(balanceOf(to, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
```

File: [2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L59 )

```solidity
59: if (from == address(0)) {
64: if (to != address(0) && to != from) {
93: if (to == address(0)) {
98: if (from != address(0) && from != to) {
```

File: [2023-01-timeswap/packages/v2-token/src/structs/Param.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L119 )

```solidity
119: if (param.long0To == address(0) || param.long1To == address(0) || param.shortTo == address(0)) Error.zeroAddress();
126: if (param.long0To == address(0) || param.long1To == address(0) || param.shortTo == address(0)) Error.zeroAddress();
133: if (param.to == address(0)) Error.zeroAddress();
140: if (param.to == address(0)) Error.zeroAddress();
147: if (param.to == address(0)) Error.zeroAddress();
```

### <a id=G4>[G-4]</a> Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate.

##### Description

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

##### Recommendation

Where appropriate, you can combine multiple address mappings into a single mapping of an address to a struct.

##### *Instances (3):*

File: [2023-01-timeswap/packages/v2-option/src/TimeswapV2OptionFactory.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L22 )

```solidity
22: mapping(address => mapping(address => address)) private optionPairs;
```

File: [2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L47 )

```solidity
47: mapping(uint256 => mapping(address => FeesPosition)) private _feesPositions;
```

File: [2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L15 )

```solidity
15: mapping(address => mapping(uint256 => uint256)) private _ownedTokens; // An index of all tokens
```

### <a id=G7>[G-5]</a> Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead.

##### Description

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

##### Recommendation

Use `uint256` instead.

##### *Instances (102):*

File: [2023-01-timeswap/packages/v2-library/src/Error.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Error.sol#L25 )

```solidity
50: error AlreadyMatured(uint256 maturity, uint96 blockTimestamp);
55: error StillActive(uint256 maturity, uint96 blockTimestamp);
106: function alreadyMatured(uint256 maturity, uint96 blockTimestamp) internal pure {
113: function stillActive(uint256 maturity, uint96 blockTimestamp) internal pure {
```

File: [2023-01-timeswap/packages/v2-library/src/Math.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Math.sol#L59 )

```solidity
59: function shr(uint256 dividend, uint8 divisorBit, bool roundUp) internal pure returns (uint256 quotient) {
70: if (value == type(uint256).max) return result = type(uint128).max;
```

File: [2023-01-timeswap/packages/v2-library/src/SafeCast.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/SafeCast.sol#L5 )

```solidity
18: function toUint16(uint256 value) internal pure returns (uint16 result) {
19: if (value > type(uint16).max) revert Uint16Overflow();
20: result = uint16(value);
27: function toUint96(uint256 value) internal pure returns (uint96 result) {
28: if (value > type(uint96).max) revert Uint96Overflow();
29: result = uint96(value);
```

File: [2023-01-timeswap/packages/v2-library/src/StrikeConversion.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/StrikeConversion.sol#L7 )

```solidity
27: return strike > type(uint128).max ? (toOne ? convert(amount, strike, true, roundUp) : amount) : (toOne ? amount : convert(amount, strike, false, roundUp));
36: return strike > type(uint128).max ? amount0 + convert(amount1, strike, false, roundUp) : amount1 + convert(amount0, strike, true, roundUp);
48: strike > type(uint128).max
```

File: [2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L70 )

```solidity
70: function blockTimestamp() internal view virtual returns (uint96) {
71: return uint96(block.timestamp); // truncation is desired
```

File: [2023-01-timeswap/packages/v2-option/src/structs/Param.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L103 )

```solidity
103: function check(TimeswapV2OptionMintParam memory param, uint96 blockTimestamp) internal pure {
105: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
117: function check(TimeswapV2OptionBurnParam memory param, uint96 blockTimestamp) internal pure {
119: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
130: function check(TimeswapV2OptionSwapParam memory param, uint96 blockTimestamp) internal pure {
132: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
143: function check(TimeswapV2OptionCollectParam memory param, uint96 blockTimestamp) internal pure {
145: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
```

File: [2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L52 )

```solidity
52: mapping(uint256 => mapping(uint256 => uint96)) private reentrancyGuards;
82: function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {
83: return uint96(block.timestamp + durationForward); // truncation is desired
247: uint96 durationForward
255: uint96 durationForward
312: uint96 durationForward
320: uint96 durationForward
372: uint96 durationForward
380: uint96 durationForward
426: function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
433: uint96 durationForward
```

File: [2023-01-timeswap/packages/v2-pool/src/TimeswapV2PoolFactory.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L20 )

```solidity
38: if (chosenTransactionFee > type(uint16).max) revert IncorrectFeeInitialization(chosenTransactionFee);
39: if (chosenProtocolFee > type(uint16).max) revert IncorrectFeeInitialization(chosenProtocolFee);
```

File: [2023-01-timeswap/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L294 )

```solidity
294: uint96 durationForward
323: uint96 durationForward
344: function deleverage(TimeswapV2PoolDeleverageParam calldata param, uint96 durationForward) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);
364: function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/ConstantProduct.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L38 )

```solidity
38: function getShort(uint160 liquidity, uint160 rate, uint96 duration, bool roundUp) internal pure returns (uint256) {
51: function calculateGivenLiquidityDelta(uint160 rate, uint160 deltaLiquidity, uint96 duration, bool isAdd) internal pure returns (uint256 longAmount, uint256 shortAmount) {
66: function calculateGivenLiquidityLong(uint160 rate, uint256 longAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 shortAmount) {
81: function calculateGivenLiquidityShort(uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 longAmount) {
101: uint96 duration,
138: uint96 duration,
168: uint96 duration,
199: uint96 duration,
234: uint96 duration,
268: function getLiquidityGivenShort(uint160 rate, uint256 shortAmount, uint96 duration, bool roundUp) private pure returns (uint160) {
313: function getNewSqrtInterestRateGivenShort(uint160 liquidity, uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) private pure returns (uint160 newRate, uint160 deltaRate) {
342: function getShortFromSqrtInterestRate(uint160 liquidity, uint160 deltaRate, uint96 duration, bool roundUp) private pure returns (uint256) {
356: function getShortOrLongFromGivenSum(uint160 liquidity, uint160 rate, uint256 sumAmount, uint96 duration, uint256 transactionFee, bool isShort) private pure returns (uint256 amount) {
373: function calculateNegativeB(uint160 liquidity, uint160 rate, uint256 sumAmount, uint96 duration, uint256 transactionFee, bool isShort) private pure returns (uint256 negativeB) {
399: uint96 duration,
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/Duration.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/Duration.sol#L11 )

```solidity
11: function init(uint256 duration) internal pure returns (uint96) {
12: if (duration > type(uint96).max) revert DurationOverflow(duration);
13: return uint96(duration);
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/DurationCalculation.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/DurationCalculation.sol#L18 )

```solidity
18: function unsafeDurationFromLastTimestampToNow(uint96 lastTimestamp, uint96 blockTimestamp) internal pure returns (uint96 duration) {
26: function unsafeDurationFromNowToMaturity(uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {
34: function unsafeDurationFromLastTimestampToMaturity(uint96 lastTimestamp, uint256 maturity) internal pure returns (uint96 duration) {
43: function unsafeDurationFromLastTimestampToNowOrMaturity(uint96 lastTimestamp, uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/Fee.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/Fee.sol#L8 )

```solidity
11: if (fee > type(uint16).max) revert IncorrectFeeInitialization(fee);
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/ReentrancyGuard.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L12 )

```solidity
12: uint96 internal constant NOT_INTERACTED = 0;
15: uint96 internal constant NOT_ENTERED = 1;
18: uint96 internal constant ENTERED = 2;
23: function check(uint96 reentrancyGuard) internal pure {
```

File: [2023-01-timeswap/packages/v2-pool/src/structs/Param.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Param.sol#L135 )

```solidity
135: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
142: function check(TimeswapV2PoolMintParam memory param, uint96 blockTimestamp) internal pure {
143: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
153: function check(TimeswapV2PoolBurnParam memory param, uint96 blockTimestamp) internal pure {
154: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
165: function check(TimeswapV2PoolDeleverageParam memory param, uint96 blockTimestamp) internal pure {
166: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
176: function check(TimeswapV2PoolLeverageParam memory param, uint96 blockTimestamp) internal pure {
177: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
188: function check(TimeswapV2PoolRebalanceParam memory param, uint96 blockTimestamp) internal pure {
189: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
```

File: [2023-01-timeswap/packages/v2-pool/src/structs/Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L43 )

```solidity
43: uint96 lastTimestamp;
101: function totalPositions(Pool storage pool, uint256 maturity, uint96 blockTimestamp) external view returns (uint256 longAmount, uint256 shortAmount) {
118: uint96 duration,
119: uint96 blockTimestamp
120: ) private pure returns (uint96 newLastTimestamp, uint256 newShortFeeGrowth) {
128: function updateDurationWeightBeforeMaturity(Pool storage pool, uint96 blockTimestamp) private {
142: function updateDurationWeightAfterMaturity(Pool storage pool, uint256 maturity, uint96 blockTimestamp) private {
158: function transferLiquidity(Pool storage pool, address to, uint160 liquidityAmount, uint96 blockTimestamp) external {
183: function transferFees(Pool storage pool, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees, uint96 blockTimestamp) external {
268: uint96 blockTimestamp
297: uint96 blockTimestamp
383: uint96 blockTimestamp
474: uint96 blockTimestamp
557: uint96 blockTimestamp
```

File: [2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L28 )

```solidity
28: using ReentrancyGuard for uint96;
41: mapping(bytes32 => uint96) private reentrancyGuards;
```

File: [2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L30 )

```solidity
30: using ReentrancyGuard for uint96;
36: mapping(bytes32 => uint96) private reentrancyGuards;
```

File: [2023-01-timeswap/packages/v2-token/src/structs/Param.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L120 )

```solidity
120: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
127: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
134: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
141: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
148: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
```

### <a id=G10>[G-6]</a> `abi.encode()` is less efficient than `abi.encodePacked()`

##### Description

`abi.encode` will apply ABI encoding rules. Therefore all elementary types are padded to 32 bytes and dynamic arrays include their length. Therefore it is possible to also decode this data again (with `abi.decode`) when the type are known.

`abi.encodePacked` will only use the only use the minimal required memory to encode the data. E.g. an address will only use 20 bytes and for dynamic arrays only the elements will be stored without length. For more info see the Solidity docs for packed mode.

For the input of the `keccak` method it is important that you can ensure that the resulting bytes of the encoding are unique. So if you always encode the same types and arrays always have the same length then there is no problem. But if you switch the parameters that you encode or encode multiple dynamic arrays you might have conflicts.
For example:
`abi.encodePacked(address(0x0000000000000000000000000000000000000001), uint(0))` encodes to the same as `abi.encodePacked(uint(0x0000000000000000000000000000000000000001000000000000000000000000), address(0))`
and `abi.encodePacked(uint[](1,2), uint[](3))` encodes to the same as `abi.encodePacked(uint[](1), uint[](2,3))`
Therefore these examples will also generate the same hashes even so they are different inputs.
On the other hand you require less memory and therefore in most cases abi.encodePacked uses less gas than abi.encode.

##### Recommendation

Use `abi.encodePacked()` where possible to save gas

##### *Instances (7):*

File: [2023-01-timeswap/packages/v2-option/src/TimeswapV2OptionDeployer.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol#L35 )

```solidity
35: optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
```

File: [2023-01-timeswap/packages/v2-pool/src/TimeswapV2PoolDeployer.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol#L28 )

```solidity
28: poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
```

File: [2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L46 )

```solidity
46: bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
53: bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
61: bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
```

File: [2023-01-timeswap/packages/v2-token/src/structs/Position.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L35 )

```solidity
35: return keccak256(abi.encode(timeswapV2TokenPosition));
40: return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
```

### <a id=G11>[G-7]</a> Internal functions only called once can be inlined to save gas.

##### Description

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.
Check this out: [link](https://betterprogramming.pub/solidity-gas-optimizations-and-tricks-2bcee0f9f1f2)
Note: To sum up, when there is an internal function that is only called once, there is no point for that function to exist.

##### Recommendation

Remove the function and make it inline if it is a simple function.

##### *Instances (2):*

File: [2023-01-timeswap/packages/v2-library/src/Ownership.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Ownership.sol#L22 )

```solidity
30: function checkIfAlreadyOwner(address chosenPendingOwner, address owner) internal pure {
38: function checkIfPendingOwner(address caller, address pendingOwner) internal pure {
```

### <a id=G12>[G-8]</a> `>=` costs less gas than `>`.

##### Description

The compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, which [saves 3 gas](https://gist.github.com/IllIllI000/3dc79d25acccfa16dee4e83ffdc6ffde)

##### Recommendation

Use `>=` instead if appropriate.

##### *Instances (36):*

File: [2023-01-timeswap/packages/v2-library/src/FullMath.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/FullMath.sol#L51 )

```solidity
51: if (addendA1 > sum1) revert AddOverflow(addendA0, addendA1, addendB0, addendB1);
68: if (subtrahend1 > minuend1 || (subtrahend1 == minuend1 && subtrahend0 > minuend0)) revert SubUnderflow(minuend0, minuend1, subtrahend0, subtrahend1);
146: if (dividend1 > productA1 || dividend0 > productA0) quotient++;
164: if (dividend1 > productA1 || dividend0 > productA0) {
277: if (value1 > product1 || value0 > product0) result++;
```

File: [2023-01-timeswap/packages/v2-library/src/SafeCast.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/SafeCast.sol#L19 )

```solidity
19: if (value > type(uint16).max) revert Uint16Overflow();
28: if (value > type(uint96).max) revert Uint96Overflow();
37: if (value > type(uint160).max) revert Uint160Overflow();
```

File: [2023-01-timeswap/packages/v2-library/src/StrikeConversion.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/StrikeConversion.sol#L27 )

```solidity
27: return strike > type(uint128).max ? (toOne ? convert(amount, strike, true, roundUp) : amount) : (toOne ? amount : convert(amount, strike, false, roundUp));
36: return strike > type(uint128).max ? amount0 + convert(amount1, strike, false, roundUp) : amount1 + convert(amount0, strike, true, roundUp);
48: strike > type(uint128).max
```

File: [2023-01-timeswap/packages/v2-option/src/structs/Param.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L105 )

```solidity
105: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
119: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
132: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
145: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
146: if (param.maturity > blockTimestamp) Error.stillActive(param.maturity, blockTimestamp);
```

File: [2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L155 )

```solidity
155: if (blockTimestamp(0) > maturity) Error.alreadyMatured(maturity, blockTimestamp(0));
```

File: [2023-01-timeswap/packages/v2-pool/src/TimeswapV2PoolFactory.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L38 )

```solidity
38: if (chosenTransactionFee > type(uint16).max) revert IncorrectFeeInitialization(chosenTransactionFee);
39: if (chosenProtocolFee > type(uint16).max) revert IncorrectFeeInitialization(chosenProtocolFee);
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/ConstantProduct.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L116 )

```solidity
116: if (isAdd ? amount < longAmount : amount > longAmount) longAmount = amount;
319: else if (rate > deltaRate) newRate = rate - deltaRate;
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/Duration.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/Duration.sol#L12 )

```solidity
12: if (duration > type(uint96).max) revert DurationOverflow(duration);
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/Fee.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/Fee.sol#L11 )

```solidity
11: if (fee > type(uint16).max) revert IncorrectFeeInitialization(fee);
```

File: [2023-01-timeswap/packages/v2-pool/src/structs/Param.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Param.sol#L135 )

```solidity
135: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
143: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
154: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
166: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
177: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
189: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
```

File: [2023-01-timeswap/packages/v2-pool/src/structs/Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L186 )

```solidity
186: if (maturity > blockTimestamp) updateDurationWeightBeforeMaturity(pool, blockTimestamp);
272: if (maturity > blockTimestamp) updateDurationWeightBeforeMaturity(pool, blockTimestamp);
```

File: [2023-01-timeswap/packages/v2-token/src/structs/Param.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L120 )

```solidity
120: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
127: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
134: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
141: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
148: if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
```
