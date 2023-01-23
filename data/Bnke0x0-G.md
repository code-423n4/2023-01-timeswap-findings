

### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gusset (20000 gas)
#### Findings:
```
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::51 => Process[] private processing;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::54 => StrikeAndMaturity[] private listOfOptions;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2OptionDeployer.sol::23 => Parameter public override parameter;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2OptionFactory.sol::25 => address[] public override getByIndex;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::55 => StrikeAndMaturity[] private listOfPools;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2PoolFactory.sol::33 => address[] public override getByIndex;
2023-01-timeswap-main/packages/v2-pool/src/base/OwnableTwoSteps.sol::14 => address public override owner;
2023-01-timeswap-main/packages/v2-pool/src/base/OwnableTwoSteps.sol::16 => address public override pendingOwner;

```




### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor avoids a separate Gsset (20000 gas).

#### Findings:
```
2023-01-timeswap-main/packages/v2-pool/src/base/OwnableTwoSteps.sol::14 => address public override owner;
2023-01-timeswap-main/packages/v2-pool/src/base/OwnableTwoSteps.sol::16 => address public override pendingOwner;
```



### [G03] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.
#### Findings:
```
2023-01-timeswap-main/packages/v2-option/src/structs/Process.sol::34 => for (uint256 i; i < processing.length; ) {
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::227 => for (uint256 i; i < ids.length; ) {
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::48 => for (uint256 i; i < ids.length; ) {
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::82 => for (uint256 i; i < ids.length; ) {
```





### [G04] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

#### Impact
> When using elements that are smaller than 32 bytes, your 
contract’s gas usage may be higher. This is because the EVM operates on 
32 bytes at a time. Therefore, if the element is smaller than that, the 
EVM must use more operations in order to reduce the size of the element 
from 32 bytes to the desired size.
> 

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed
#### Findings:
```
2023-01-timeswap-main/packages/v2-library/src/Error.sol::83 => function alreadyHaveLiquidity(uint160 liquidity) internal pure {
2023-01-timeswap-main/packages/v2-library/src/SafeCast.sol::20 => result = uint16(value);
2023-01-timeswap-main/packages/v2-library/src/SafeCast.sol::29 => result = uint96(value);
2023-01-timeswap-main/packages/v2-library/src/SafeCast.sol::38 => result = uint160(value);
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::70 => function blockTimestamp() internal view virtual returns (uint96) {
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::71 => return uint96(block.timestamp); // truncation is desired
2023-01-timeswap-main/packages/v2-option/src/structs/Param.sol::103 => function check(TimeswapV2OptionMintParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-option/src/structs/Param.sol::117 => function check(TimeswapV2OptionBurnParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-option/src/structs/Param.sol::130 => function check(TimeswapV2OptionSwapParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-option/src/structs/Param.sol::143 => function check(TimeswapV2OptionCollectParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::247 => uint96 durationForward
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::255 => uint96 durationForward
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::312 => uint96 durationForward
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::320 => uint96 durationForward
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::372 => uint96 durationForward
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::380 => uint96 durationForward
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::433 => uint96 durationForward
2023-01-timeswap-main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol::294 => uint96 durationForward
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::99 => uint160 rate,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::101 => uint96 duration,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::135 => uint160 liquidity,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::136 => uint160 rate,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::137 => uint160 deltaRate,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::138 => uint96 duration,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::165 => uint160 liquidity,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::166 => uint160 rate,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::168 => uint96 duration,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::196 => uint160 liquidity,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::197 => uint160 rate,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::199 => uint96 duration,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::205 => uint160 deltaRate;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::231 => uint160 liquidity,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::232 => uint160 rate,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::234 => uint96 duration,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::396 => uint160 liquidity,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::397 => uint160 rate,
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::399 => uint96 duration,
2023-01-timeswap-main/packages/v2-pool/src/libraries/Duration.sol::13 => return uint96(duration);
2023-01-timeswap-main/packages/v2-pool/src/libraries/DurationCalculation.sol::18 => function unsafeDurationFromLastTimestampToNow(uint96 lastTimestamp, uint96 blockTimestamp) internal pure returns (uint96 duration) {
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::12 => uint96 internal constant NOT_INTERACTED = 0;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::15 => uint96 internal constant NOT_ENTERED = 1;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::18 => uint96 internal constant ENTERED = 2;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::23 => function check(uint96 reentrancyGuard) internal pure {
2023-01-timeswap-main/packages/v2-pool/src/structs/CallbackParam.sol::16 => uint160 liquidityAmount;
2023-01-timeswap-main/packages/v2-pool/src/structs/CallbackParam.sol::34 => uint160 liquidityAmount;
2023-01-timeswap-main/packages/v2-pool/src/structs/CallbackParam.sol::54 => uint160 liquidityAmount;
2023-01-timeswap-main/packages/v2-pool/src/structs/CallbackParam.sol::72 => uint160 liquidityAmount;
2023-01-timeswap-main/packages/v2-pool/src/structs/LiquidityPosition.sol::16 => uint160 liquidity;
2023-01-timeswap-main/packages/v2-pool/src/structs/Param.sol::142 => function check(TimeswapV2PoolMintParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-pool/src/structs/Param.sol::153 => function check(TimeswapV2PoolBurnParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-pool/src/structs/Param.sol::165 => function check(TimeswapV2PoolDeleverageParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-pool/src/structs/Param.sol::176 => function check(TimeswapV2PoolLeverageParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-pool/src/structs/Param.sol::188 => function check(TimeswapV2PoolRebalanceParam memory param, uint96 blockTimestamp) internal pure {
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::42 => uint160 liquidity;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::43 => uint96 lastTimestamp;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::44 => uint160 sqrtInterestRate;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::115 => uint160 liquidity,
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::116 => uint160 rate,
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::118 => uint96 duration,
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::119 => uint96 blockTimestamp
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::158 => function transferLiquidity(Pool storage pool, address to, uint160 liquidityAmount, uint96 blockTimestamp) external {
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::205 => function initialize(Pool storage pool, uint160 rate) external {
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::268 => uint96 blockTimestamp
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::297 => uint96 blockTimestamp
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::383 => uint96 blockTimestamp
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::474 => uint96 blockTimestamp
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::557 => uint96 blockTimestamp
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::28 => using ReentrancyGuard for uint96;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::36 => constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::41 => mapping(bytes32 => uint96) private reentrancyGuards;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::70 => function transferTokenPositionFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata timeswapV2LiquidityTokenPosition, uint160 liquidityAmount) external {
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::125 => uint160 liquidityBalanceTarget = ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)) + param.liquidityAmount;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::249 => if (from != address(0)) _feesPositions[id][from].update(uint160(balanceOf(from, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::251 => if (to != address(0)) _feesPositions[id][to].update(uint160(balanceOf(to, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::30 => using ReentrancyGuard for uint96;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::36 => mapping(bytes32 => uint96) private reentrancyGuards;
2023-01-timeswap-main/packages/v2-token/src/structs/CallbackParam.sol::55 => uint160 liquidityAmount;
2023-01-timeswap-main/packages/v2-token/src/structs/CallbackParam.sol::70 => uint160 liquidityAmount;
2023-01-timeswap-main/packages/v2-token/src/structs/Param.sol::72 => uint160 liquidityAmount;
2023-01-timeswap-main/packages/v2-token/src/structs/Param.sol::90 => uint160 liquidityAmount;
```





### [G05] Use custom errors rather than `revert()`/`require()` strings to save deployment gas


#### Findings:
```
2023-01-timeswap-main/packages/v2-library/src/BytesLib.sol::13 => require(_length + 31 >= _length, "slice_overflow");
2023-01-timeswap-main/packages/v2-library/src/BytesLib.sol::14 => require(_bytes.length >= _start + _length, "slice_outOfBounds");
```




### [G06] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.10 to have external calls skip
 contract existence checks if the external call has a return value
#### Findings:
```
2023-01-timeswap-main/packages/v2-library/src/BytesLib.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/CatchError.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/Error.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/FullMath.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/Math.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/Ownership.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/SafeCast.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/StrikeConversion.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2OptionDeployer.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2OptionFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/enums/Position.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/enums/Transaction.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/NoDelegateCall.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/libraries/OptionFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/libraries/OptionPair.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/libraries/Proportion.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/CallbackParam.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/Option.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/Param.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/Process.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/StrikeAndMaturity.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/NoDelegateCall.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2PoolFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/base/OwnableTwoSteps.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/enums/Transaction.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantSum.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/Duration.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/DurationCalculation.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/DurationWeight.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/Fee.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/FeeCalculation.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/PoolFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/PoolPair.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/structs/CallbackParam.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/structs/LiquidityPosition.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/structs/Param.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap-main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap-main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2TokenMintCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/CallbackParam.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/FeesPosition.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/Param.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/Position.sol::2 => pragma solidity ^0.8.8;
```



### [G07] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
Saves a storage slot for the mapping. Depending on the circumstances 
and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires
 both values and they both fit in the same storage slot
#### Findings:
```
2023-01-timeswap-main/packages/v2-option/src/structs/Option.sol::23 => mapping(address => uint256) long0;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::41 => mapping(bytes32 => uint96) private reentrancyGuards;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::39 => mapping(bytes32 => uint256) private _timeswapV2TokenPositionIds;
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::17 => mapping(address => uint256) private _currentIndex; // the current index for an address
```


### [G08] Using `bools` for storage incurs overhead


#### Findings:
```
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::53 => mapping(uint256 => mapping(uint256 => bool)) private hasInteracted;
```




### [G09] `abi.encode()` is less efficient than abi.encodePacked()


#### Findings:
```
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2OptionDeployer.sol::35 => optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol::28 => poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::46 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::53 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::61 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap-main/packages/v2-token/src/structs/Position.sol::35 => return keccak256(abi.encode(timeswapV2TokenPosition));
2023-01-timeswap-main/packages/v2-token/src/structs/Position.sol::40 => return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
```




### [G10] Optimize names to save gas

#### Impact
public/external function names and public member variable names can be optimized to save gas. 

#### Findings:
```
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::13 => abstract contract ERC1155Enumerable is IERC1155Enumerable, ERC1155 {
```



