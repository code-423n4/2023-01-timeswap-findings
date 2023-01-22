# QA REPORT

---

### Summary of optimizations


|              | Issue                                                                                                         | Instances |
| -------------- | --------------------------------------------------------------------------------------------------------------- | :---------: |
| [L-1](#G13) | Use of`block.timestamp`                                                                                 |     2     |
| [N-1](#G15) | Lines are too long.                                                                                     |    109    |
| [N-2](#G16) | For modern and more readable code, update import usages.                                                |     2     |

*Total: 113 Instances over 3 issues.*


---

### Low and Non Critical risk issues

### <a id=G13>[L-1]</a> Use of `block.timestamp`

##### Description

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the *[Entropy Illusion](https://hacken.io/discover/most-common-smart-contract-vulnerabilities/#Entropy_Illusion)* for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

##### Recommendation

Use oracle instead of `block.timestamp`

##### *Instances (2):*

File: [2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L71 )

```solidity
71: return uint96(block.timestamp); // truncation is desired
```

File: [2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L83 )

```solidity
83: return uint96(block.timestamp + durationForward); // truncation is desired
```

### <a id=G15>[N-1]</a> Lines are too long.

##### Description

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

##### Recommendation

Reduce number of characters per line to improve readability.

##### *Instances (109):*

File: [2023-01-timeswap/packages/v2-library/src/FullMath.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/FullMath.sol#L62 )

```solidity
62: function sub512(uint256 minuend0, uint256 minuend1, uint256 subtrahend0, uint256 subtrahend1) internal pure returns (uint256 difference0, uint256 difference1) {
```

File: [2023-01-timeswap/packages/v2-option/src/TimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L27 )

```solidity
27: import {TimeswapV2OptionMintCallbackParam, TimeswapV2OptionBurnCallbackParam, TimeswapV2OptionSwapCallbackParam, TimeswapV2OptionCollectCallbackParam} from "./structs/CallbackParam.sol";
118: (token0AndLong0Amount, token1AndLong1Amount, shortAmount) = option.mint(param.strike, param.long0To, param.long1To, param.shortTo, param.transaction, param.amount0, param.amount1);
198: function swap(TimeswapV2OptionSwapParam calldata param) external override noDelegateCall returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, bytes memory data) {
235: Error.checkEnough(IERC20(param.isLong0ToLong1 ? token1 : token0).balanceOf(address(this)), param.isLong0ToLong1 ? currentProcess.balance1Target : currentProcess.balance0Target);
246: function collect(TimeswapV2OptionCollectParam calldata param) external override noDelegateCall returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount, bytes memory data) {
```

File: [2023-01-timeswap/packages/v2-option/src/interfaces/ITimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L159 )

```solidity
159: function mint(TimeswapV2OptionMintParam calldata param) external returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data);
169: function burn(TimeswapV2OptionBurnParam calldata param) external returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data);
190: function collect(TimeswapV2OptionCollectParam calldata param) external returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount, bytes memory data);
```

File: [2023-01-timeswap/packages/v2-option/src/structs/Option.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L95 )

```solidity
95: if (transaction == TimeswapV2OptionMint.GivenTokensAndLongs) shortAmount = StrikeConversion.combine(token0AndLong0Amount = amount0, token1AndLong1Amount = amount1, strike, false);
134: if (transaction == TimeswapV2OptionBurn.GivenTokensAndLongs) shortAmount = StrikeConversion.combine(token0AndLong0Amount = amount0, token1AndLong1Amount = amount1, strike, true);
191: function collect(Option storage option, uint256 strike, TimeswapV2OptionCollect transaction, uint256 amount) internal returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount) {
```

File: [2023-01-timeswap/packages/v2-pool/src/TimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L31 )

```solidity
31: import {TimeswapV2PoolCollectParam, TimeswapV2PoolMintParam, TimeswapV2PoolBurnParam, TimeswapV2PoolDeleverageParam, TimeswapV2PoolLeverageParam, TimeswapV2PoolRebalanceParam, ParamLibrary} from "./structs/Param.sol";
32: import {TimeswapV2PoolMintChoiceCallbackParam, TimeswapV2PoolMintCallbackParam, TimeswapV2PoolBurnChoiceCallbackParam, TimeswapV2PoolBurnCallbackParam, TimeswapV2PoolDeleverageChoiceCallbackParam, TimeswapV2PoolDeleverageCallbackParam, TimeswapV2PoolLeverageCallbackParam, TimeswapV2PoolLeverageChoiceCallbackParam, TimeswapV2PoolRebalanceCallbackParam} from "./structs/CallbackParam.sol";
34: import {TimeswapV2PoolMint, TimeswapV2PoolBurn, TimeswapV2PoolDeleverage, TimeswapV2PoolLeverage, TimeswapV2PoolRebalance, TransactionLibrary} from "./enums/Transaction.sol";
123: function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view override returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
128: function protocolFeesEarned(uint256 strike, uint256 maturity) external view override returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {
184: function collectProtocolFees(TimeswapV2PoolCollectParam calldata param) external override noDelegateCall returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) {
192: (long0Amount, long1Amount, shortAmount) = pools[param.strike][param.maturity].collectProtocolFees(param.long0Requested, param.long1Requested, param.shortRequested);
202: function collectTransactionFees(TimeswapV2PoolCollectParam calldata param) external override noDelegateCall returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) {
231: function collect(uint256 strike, uint256 maturity, address long0To, address long1To, address shortTo, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) private {
240: function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
267: if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;
271: if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;
274: uint256 shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;
293: if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);
295: if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);
297: Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);
305: function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
335: if (long0Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long0To, TimeswapV2OptionPosition.Long0, long0Amount);
338: if (long1Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long1To, TimeswapV2OptionPosition.Long1, long1Amount);
365: function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
387: (long0Amount, long1Amount, shortAmount, data) = pools[param.strike][param.maturity].deleverage(param, transactionFee, protocolFee, blockTimestamp(durationForward));
393: if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;
397: if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;
404: TimeswapV2PoolDeleverageCallbackParam({strike: param.strike, maturity: param.maturity, long0Amount: long0Amount, long1Amount: long1Amount, shortAmount: shortAmount, data: data})
411: if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);
413: if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);
421: function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
426: function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
440: (long0Amount, long1Amount, shortAmount, data) = pools[param.strike][param.maturity].leverage(param, transactionFee, protocolFee, blockTimestamp(durationForward));
444: uint256 balanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;
448: if (long0Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long0To, TimeswapV2OptionPosition.Long0, long0Amount);
450: if (long1Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long1To, TimeswapV2OptionPosition.Long1, long1Amount);
454: TimeswapV2PoolLeverageCallbackParam({strike: param.strike, maturity: param.maturity, long0Amount: long0Amount, long1Amount: long1Amount, shortAmount: shortAmount, data: data})
469: function rebalance(TimeswapV2PoolRebalanceParam calldata param) external override noDelegateCall returns (uint256 long0Amount, uint256 long1Amount, bytes memory data) {
511: ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), param.isLong0ToLong1 ? TimeswapV2OptionPosition.Long0 : TimeswapV2OptionPosition.Long1),
```

File: [2023-01-timeswap/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L6 )

```solidity
6: import {TimeswapV2PoolCollectParam, TimeswapV2PoolMintParam, TimeswapV2PoolBurnParam, TimeswapV2PoolDeleverageParam, TimeswapV2PoolLeverageParam, TimeswapV2PoolRebalanceParam} from "../structs/Param.sol";
26: event TransferFees(uint256 indexed strike, uint256 indexed maturity, address indexed from, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees);
81: event Mint(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);
115: event Deleverage(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);
126: event Leverage(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address long0To, address long1To, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);
138: event Rebalance(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, bool isLong0ToLong1, uint256 long0Amount, uint256 long1Amount);
204: function protocolFeesEarned(uint256 strike, uint256 maturity) external view returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees);
280: function mint(TimeswapV2PoolMintParam calldata param) external returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);
307: function burn(TimeswapV2PoolBurnParam calldata param) external returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);
333: function deleverage(TimeswapV2PoolDeleverageParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);
344: function deleverage(TimeswapV2PoolDeleverageParam calldata param, uint96 durationForward) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);
353: function leverage(TimeswapV2PoolLeverageParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);
364: function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);
```

File: [2023-01-timeswap/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol#L13 )

```solidity
13: function timeswapV2PoolBurnChoiceCallback(TimeswapV2PoolBurnChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);
```

File: [2023-01-timeswap/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L14 )

```solidity
14: function timeswapV2PoolDeleverageChoiceCallback(TimeswapV2PoolDeleverageChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);
```

File: [2023-01-timeswap/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L14 )

```solidity
14: function timeswapV2PoolLeverageChoiceCallback(TimeswapV2PoolLeverageChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);
```

File: [2023-01-timeswap/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L14 )

```solidity
14: function timeswapV2PoolMintChoiceCallback(TimeswapV2PoolMintChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/ConstantProduct.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L51 )

```solidity
51: function calculateGivenLiquidityDelta(uint160 rate, uint160 deltaLiquidity, uint96 duration, bool isAdd) internal pure returns (uint256 longAmount, uint256 shortAmount) {
66: function calculateGivenLiquidityLong(uint160 rate, uint256 longAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 shortAmount) {
81: function calculateGivenLiquidityShort(uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 longAmount) {
313: function getNewSqrtInterestRateGivenShort(uint160 liquidity, uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) private pure returns (uint160 newRate, uint160 deltaRate) {
334: return roundUp ? FullMath.mulDiv(numerator, deltaRate, uint256(rate), true).div(newRate, true) : FullMath.mulDiv(numerator, deltaRate, uint256(newRate), false).div(rate, false);
356: function getShortOrLongFromGivenSum(uint160 liquidity, uint160 rate, uint256 sumAmount, uint96 duration, uint256 transactionFee, bool isShort) private pure returns (uint256 amount) {
373: function calculateNegativeB(uint160 liquidity, uint160 rate, uint256 sumAmount, uint96 duration, uint256 transactionFee, bool isShort) private pure returns (uint256 negativeB) {
404: uint256 denominator = isShort ? (uint256(1) << 174).unsafeMul((uint256(1) << 16).unsafeSub(transactionFee)) : uint256(rate).unsafeMul((uint256(1) << 16).unsafeSub(transactionFee));
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/ConstantSum.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantSum.sol#L19 )

```solidity
19: function calculateGivenLongIn(uint256 strike, uint256 longAmountIn, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountOut, uint256 longFees) {
30: function calculateGivenLongOut(uint256 strike, uint256 longAmountOut, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountIn, uint256 longFees) {
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/DurationCalculation.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/DurationCalculation.sol#L43 )

```solidity
43: function unsafeDurationFromLastTimestampToNowOrMaturity(uint96 lastTimestamp, uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/FeeCalculation.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L27 )

```solidity
27: function update(uint160 liquidity, uint256 feeGrowth, uint256 protocolFees, uint256 fees, uint256 protocolFee) internal pure returns (uint256 newFeeGrowth, uint256 newProtocolFees) {
```

File: [2023-01-timeswap/packages/v2-pool/src/libraries/PoolFactory.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/PoolFactory.sol#L43 )

```solidity
43: function getWithCheck(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {
```

File: [2023-01-timeswap/packages/v2-pool/src/structs/Param.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Param.sol#L6 )

```solidity
6: import {TimeswapV2PoolMint, TimeswapV2PoolBurn, TimeswapV2PoolDeleverage, TimeswapV2PoolLeverage, TimeswapV2PoolRebalance, TransactionLibrary} from "../enums/Transaction.sol";
```

File: [2023-01-timeswap/packages/v2-pool/src/structs/Pool.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L25 )

```solidity
25: import {TimeswapV2PoolMint, TimeswapV2PoolBurn, TimeswapV2PoolDeleverage, TimeswapV2PoolLeverage, TimeswapV2PoolRebalance, TransactionLibrary} from "../enums/Transaction.sol";
27: import {TimeswapV2PoolCollectParam, TimeswapV2PoolMintParam, TimeswapV2PoolBurnParam, TimeswapV2PoolDeleverageParam, TimeswapV2PoolLeverageParam, TimeswapV2PoolRebalanceParam} from "./Param.sol";
28: import {TimeswapV2PoolMintChoiceCallbackParam, TimeswapV2PoolBurnChoiceCallbackParam, TimeswapV2PoolDeleverageChoiceCallbackParam, TimeswapV2PoolLeverageChoiceCallbackParam} from "./CallbackParam.sol";
103: shortAmount = ConstantProduct.getShort(pool.liquidity, pool.sqrtInterestRate, DurationCalculation.unsafeDurationFromNowToMaturity(maturity, blockTimestamp), false);
183: function transferFees(Pool storage pool, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees, uint96 blockTimestamp) external {
533: TimeswapV2PoolDeleverageChoiceCallbackParam({strike: param.strike, maturity: param.maturity, longAmount: longAmount, shortAmount: shortAmount, data: param.data})
639: (pool.long0FeeGrowth, pool.long0ProtocolFees) = FeeCalculation.update(pool.liquidity, pool.long0FeeGrowth, pool.long0ProtocolFees, long0Fees, protocolFee);
653: (pool.long1FeeGrowth, pool.long1ProtocolFees) = FeeCalculation.update(pool.liquidity, pool.long1FeeGrowth, pool.long1ProtocolFees, long1Fees, protocolFee);
665: function rebalance(Pool storage pool, TimeswapV2PoolRebalanceParam memory param, uint256 transactionFee, uint256 protocolFee) external returns (uint256 long0Amount, uint256 long1Amount) {
697: (pool.long1FeeGrowth, pool.long1ProtocolFees) = FeeCalculation.update(pool.liquidity, pool.long1FeeGrowth, pool.long1ProtocolFees, longFees, protocolFee);
724: (pool.long0FeeGrowth, pool.long0ProtocolFees) = FeeCalculation.update(pool.liquidity, pool.long0FeeGrowth, pool.long0ProtocolFees, longFees, protocolFee);
```

File: [2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L22 )

```solidity
22: import {TimeswapV2LiquidityTokenMintCallbackParam, TimeswapV2LiquidityTokenBurnCallbackParam, TimeswapV2LiquidityTokenCollectCallbackParam} from "./structs/CallbackParam.sol";
70: function transferTokenPositionFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata timeswapV2LiquidityTokenPosition, uint160 liquidityAmount) external {
75: function transferFeesFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external override {
182: function collect(TimeswapV2LiquidityTokenCollectParam calldata param) external returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees, bytes memory data) {
224: function _beforeTokenTransfer(address operator, address from, address to, uint256[] memory ids, uint256[] memory amounts, bytes memory data) internal override {
244: (, address poolPair) = PoolFactoryLibrary.getWithCheck(optionFactory, poolFactory, timeswapV2LiquidityTokenPosition.token0, timeswapV2LiquidityTokenPosition.token1);
246: (long0FeeGrowth, long1FeeGrowth, shortFeeGrowth) = ITimeswapV2Pool(poolPair).feeGrowth(timeswapV2LiquidityTokenPosition.strike, timeswapV2LiquidityTokenPosition.maturity);
270: (, address poolPair) = PoolFactoryLibrary.getWithCheck(optionFactory, poolFactory, timeswapV2LiquidityTokenPosition.token0, timeswapV2LiquidityTokenPosition.token1);
272: (long0FeeGrowth, long1FeeGrowth, shortFeeGrowth) = ITimeswapV2Pool(poolPair).feeGrowth(timeswapV2LiquidityTokenPosition.strike, timeswapV2LiquidityTokenPosition.maturity);
277: (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) = feesPosition.feesEarnedOf(uint160(balanceOf(owner, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
```

File: [2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L87 )

```solidity
87: long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + param.long0Amount;
117: long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + param.long1Amount;
146: shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + param.shortAmount;
186: if (param.long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);
189: if (param.long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);
192: if (param.shortAmount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);
205: if (param.long0Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long0To, TimeswapV2OptionPosition.Long0, param.long0Amount);
213: if (param.shortAmount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.shortTo, TimeswapV2OptionPosition.Short, param.shortAmount);
```

File: [2023-01-timeswap/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L42 )

```solidity
42: function transferFeesFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external;
59: function collect(TimeswapV2LiquidityTokenCollectParam calldata param) external returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees, bytes memory data);
```

### <a id=G16>[N-2]</a> For modern and more readable code, update import usages.

##### Description

Solidity code is cleaner in the following way: On the principle that clearer code is better code, you should import the things you want to use. Specific imports with curly braces allow us to apply this rule better. Check out this [article](https://betterprogramming.pub/solidity-tutorial-all-about-imports-c65110e41f3a)

##### Recommendation

Import like this: `import {contract1 , contrat2c} from "filename.sol";`

##### *Instances (2):*

File: [2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L5 )

```solidity
5: import "forge-std/console.sol";
```

File: [2023-01-timeswap/packages/v2-token/src/interfaces/IERC1155Enumerable.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L6 )

```solidity
6: import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
```