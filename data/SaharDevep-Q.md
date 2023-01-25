# Audit Report

## Summary
[L01] UNSPECIFIC COMPILER VERSION
[L02] USE A MORE RECENT VERSION OF SOLIDITY
[N01] LINES ARE TOO LONG
[N02] NOT USING THE NAMED RETURN VARIABLES ANYWHERE IN THE FUNCTION IS CONFUSING

## Issues found

### [L01] Unspecific Compiler Version Pragma
In some files the pragma is locked in 0.8.8 but in some other the pragma is floating. Solidity pragma versioning should be exactly same in all contracts.

#### Impact
Issue Information:(https://github.com/byterocket/c4-common-issues/blob/main/2-Low-Risk.md#l003---unspecific-compiler-version-pragma)

#### Findings:
```
packages\v2-token\src\TimeswapV2LiquidityToken.sol::2 => pragma solidity ^0.8.8;
packages\v2-token\src\TimeswapV2Token.sol::2 => pragma solidity ^0.8.8;
packages\v2-token\src\base\ERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
packages\v2-token\src\interfaces\IERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
packages\v2-token\src\structs\CallbackParam.sol::2 => pragma solidity ^0.8.8;
packages\v2-token\src\structs\FeesPosition.sol::2 => pragma solidity ^0.8.8;
packages\v2-token\src\structs\Param.sol::2 => pragma solidity ^0.8.8;
packages\v2-token\src\structs\Position.sol::2 => pragma solidity ^0.8.8;
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)


### [L02] USE A MORE RECENT VERSION OF SOLIDITY
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked()
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.

#### Tools used
Manual

### [N01] Lines are too long

Lines length should not be more than 120. Break the lines for better readabality.

#### Findings:
```
packages\v2-library\src\FullMath.sol::46 => function add512(uint256 addendA0, uint256 addendA1, uint256 addendB0, uint256 addendB1) internal pure returns (uint256 sum0, uint256 sum1) {
packages\v2-library\src\FullMath.sol::62 => function sub512(uint256 minuend0, uint256 minuend1, uint256 subtrahend0, uint256 subtrahend1) internal pure returns (uint256 difference0, uint256 difference1) {
packages\v2-library\src\FullMath.sol::68 => if (subtrahend1 > minuend1 || (subtrahend1 == minuend1 && subtrahend0 > minuend0)) revert SubUnderflow(minuend0, minuend1, subtrahend0, subtrahend1);
packages\v2-library\src\FullMath.sol::77 => function mul512(uint256 multiplicand, uint256 multiplier) internal pure returns (uint256 product0, uint256 product1) {
packages\v2-library\src\FullMath.sol::115 => function div512(uint256 dividend0, uint256 dividend1, uint256 divisor) private pure returns (uint256 quotient0, uint256 quotient1) {
packages\v2-library\src\FullMath.sol::138 => function div512To256(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
packages\v2-library\src\FullMath.sol::158 => function div512(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient0, uint256 quotient1) {
packages\v2-library\src\FullMath.sol::181 => function mulDiv(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) internal pure returns (uint256 result) {
packages\v2-library\src\FullMath.sol::287 => function sqrt512Estimate(uint256 value0, uint256 value1, uint256 currentEstimate) private pure returns (uint256 estimate) {
packages\v2-library\src\StrikeConversion.sol::17 => return zeroToOne ? FullMath.mulDiv(amount, strike, uint256(1) << 128, roundUp) : FullMath.mulDiv(amount, uint256(1) << 128, strike, roundUp);
packages\v2-library\src\StrikeConversion.sol::27 => return strike > type(uint128).max ? (toOne ? convert(amount, strike, true, roundUp) : amount) : (toOne ? amount : convert(amount, strike, false, roundUp));
packages\v2-library\src\StrikeConversion.sol::36 => return strike > type(uint128).max ? amount0 + convert(amount1, strike, false, roundUp) : amount1 + convert(amount0, strike, true, roundUp);
packages\v2-library\src\StrikeConversion.sol::46 => function dif(uint256 base, uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) internal pure returns (uint256) {
packages\v2-library\src\StrikeConversion.sol::49-50 => ? (zeroToOne ? convert(base - amount, strike, true, roundUp) : base - convert(amount, strike, false, !roundUp))
packages\v2-option\src\TimeswapV2Option.sol::85 => function totalPosition(uint256 strike, uint256 maturity, TimeswapV2OptionPosition position) external view override returns (uint256) {
packages\v2-option\src\TimeswapV2Option.sol::90 => function positionOf(uint256 strike, uint256 maturity, address owner, TimeswapV2OptionPosition position) external view override returns (uint256) {
packages\v2-option\src\TimeswapV2Option.sol::97 => function transferPosition(uint256 strike, uint256 maturity, address to, TimeswapV2OptionPosition position, uint256 amount) external override {
packages\v2-option\src\TimeswapV2Option.sol::111 => ) external override noDelegateCall returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-option\src\TimeswapV2Option.sol::118 => (token0AndLong0Amount, token1AndLong1Amount, shortAmount) = option.mint(param.strike, param.long0To, param.long1To, param.shortTo, param.transaction, param.amount0, param.amount1);
packages\v2-option\src\TimeswapV2Option.sol::153 => emit Mint(param.strike, param.maturity, msg.sender, param.long0To, param.long1To, param.shortTo, token0AndLong0Amount, token1AndLong1Amount, shortAmount);
packages\v2-option\src\TimeswapV2Option.sol::159 => ) external override noDelegateCall returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-option\src\TimeswapV2Option.sol::166 => (token0AndLong0Amount, token1AndLong1Amount, shortAmount) = option.burn(param.strike, param.transaction, param.amount0, param.amount1);
packages\v2-option\src\TimeswapV2Option.sol::194 => emit Burn(param.strike, param.maturity, msg.sender, param.token0To, param.token1To, token0AndLong0Amount, token1AndLong1Amount, shortAmount);
packages\v2-option\src\TimeswapV2Option.sol::198 => function swap(TimeswapV2OptionSwapParam calldata param) external override noDelegateCall returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, bytes memory data) {
packages\v2-option\src\TimeswapV2Option.sol::205 => (token0AndLong0Amount, token1AndLong1Amount) = option.swap(param.strike, param.longTo, param.isLong0ToLong1, param.transaction, param.amount);
packages\v2-option\src\TimeswapV2Option.sol::215-116 => param.isLong0ToLong1 ? IERC20(token0).balanceOf(address(this)) - token0AndLong0Amount : IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
packages\v2-option\src\TimeswapV2Option.sol::220 => IERC20(param.isLong0ToLong1 ? token0 : token1).safeTransfer(param.tokenTo, param.isLong0ToLong1 ? token0AndLong0Amount : token1AndLong1Amount);
packages\v2-option\src\TimeswapV2Option.sol::234 => Error.checkEnough(IERC20(param.isLong0ToLong1 ? token1 : token0).balanceOf(address(this)), param.isLong0ToLong1 ? currentProcess.balance1Target : currentProcess.balance0Target);
packages\v2-option\src\TimeswapV2Option.sol::243 => emit Swap(param.strike, param.maturity, msg.sender, param.tokenTo, param.longTo, param.isLong0ToLong1, token0AndLong0Amount, token1AndLong1Amount);}
etc...
```

#### Tools used
Manual


### [N02] NOT USING THE NAMED RETURN VARIABLES ANYWHERE IN THE FUNCTION IS CONFUSING

#### Findings:
```
packages\v2-pool\src\TimeswapV2Pool.sol::118 => function feeGrowth(uint256 strike, uint256 maturity) external view override returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {
packages\v2-pool\src\TimeswapV2Pool.sol::123 => function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view override returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
packages\v2-pool\src\TimeswapV2Pool.sol::128 => function protocolFeesEarned(uint256 strike, uint256 maturity) external view override returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {
packages\v2-pool\src\TimeswapV2Pool.sol::240 => function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::248 => ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::305 => function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::313 =>  ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::365 => function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::373 => ) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::421 => function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::426 => function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\structs\Pool.sol::75 => function feesEarnedOf(Pool storage pool, address owner) external view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
```

#### Tools used
Manual

### [N03] Misleading comment/code
We use unchecked for the operations that do not underflow/overflow. But the comment says that the code may underflow/overflow.

#### Findings:
```
packages\v2-library\src\Math.sol::14 => function unsafeAdd(uint256 addend1, uint256 addend2) internal pure returns (uint256 sum) {
packages\v2-library\src\Math.sol::25 => function unsafeSub(uint256 minuend, uint256 subtrahend) internal pure returns (uint256 difference) {
packages\v2-library\src\Math.sol::36 => function unsafeMul(uint256 multiplicand, uint256 multiplier) internal pure returns (uint256 product) {
```

#### Tools used
Manual
