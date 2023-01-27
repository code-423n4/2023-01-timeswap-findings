[G-01] NOT USING THE NAMED RETURN VARIABLES WHEN A FUNCTION RETURNS, WASTES DEPLOYMENT GAS
It is not necessary to have both a named return and a return statement.
Proof Of Concept
v2-library/src/BytesLib.sol
12: function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L12

v2-library/src/SafeCast.sol
18: function toUint16(uint256 value) internal pure returns (uint16 result) {
27: function toUint96(uint256 value) internal pure returns (uint96 result) {
36: function toUint160(uint256 value) internal pure returns (uint160 result) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/SafeCast.sol#L18
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/SafeCast.sol#L27
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/SafeCast.sol#L36

v2-library/src/FullMath.sol
46: function add512(uint256 addendA0, uint256 addendA1, uint256 addendB0, uint256 addendB1) internal pure returns (uint256 sum0, uint256 sum1) {
62: function sub512(uint256 minuend0, uint256 minuend1, uint256 subtrahend0, uint256 subtrahend1) internal pure returns (uint256 difference0, uint256 difference1) {
77: function mul512(uint256 multiplicand, uint256 multiplier) internal pure returns (uint256 product0, uint256 product1) {
90: function div256(uint256 divisor) private pure returns (uint256 quotient) {
101: function mod256(uint256 value) private pure returns (uint256 result) {
115: function div512(uint256 dividend0, uint256 dividend1, uint256 divisor) private pure returns (uint256 quotient0, uint256 quotient1) {
138:  function div512To256(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
158: function div512(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient0, uint256 quotient1) {
181: function mulDiv(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) internal pure returns (uint256 result) {
265: function sqrt512(uint256 value0, uint256 value1, bool roundUp) internal pure returns (uint256 result) {
287: function sqrt512Estimate(uint256 value0, uint256 value1, uint256 currentEstimate) private pure returns (uint256 estimate) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L46
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L62
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L77
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L90
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L101
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L115
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L138
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L158
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L181
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L265
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L287

v2-library/src/Math.sol
14: function unsafeAdd(uint256 addend1, uint256 addend2) internal pure returns (uint256 sum) {
25: function unsafeSub(uint256 minuend, uint256 subtrahend) internal pure returns (uint256 difference) {
36: function unsafeMul(uint256 multiplicand, uint256 multiplier) internal pure returns (uint256 product) {
48: function div(uint256 dividend, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
59: function shr(uint256 dividend, uint8 divisorBit, bool roundUp) internal pure returns (uint256 quotient) {
69: function sqrt(uint256 value, bool roundUp) internal pure returns (uint256 result) {
88: function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L14
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L25
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L36
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L48
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L59
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L69
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L88

v2-library/src/CatchError.sol
12: function catchError(bytes memory reason, bytes4 selector) internal pure returns (bytes memory) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/CatchError.sol#L12

v2-pool/src/TimeswapV2Pool.sol
118: function feeGrowth(uint256 strike, uint256 maturity) external view override returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {
123: function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view override returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
128: function protocolFeesEarned(uint256 strike, uint256 maturity) external view override returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {
133: function totalLongBalance(uint256 strike, uint256 maturity) external view override returns (uint256 long0Amount, uint256 long1Amount) {
140: function totalLongBalanceAdjustFees(uint256 strike, uint256 maturity) external view override returns (uint256 long0Amount, uint256 long1Amount) {
145: function totalPositions(uint256 strike, uint256 maturity) external view override returns (uint256 longAmount, uint256 shortAmount) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L118
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L123
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L128
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L133
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L140
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L145

v2-pool/src/TimeswapV2PoolFactory.sol
48: function get(address optionPair) external view override returns (address pair) {
59: function create(address optionPair) external override returns (address pair) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L48
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L59

v2-pool/src/interfaces/ITimeswapV2Pool.sol
167: function totalLiquidity(uint256 strike, uint256 maturity) external view returns (uint160 liquidityAmount);
174: function sqrtInterestRate(uint256 strike, uint256 maturity) external view returns (uint160 rate);
181: function liquidityOf(uint256 strike, uint256 maturity, address owner) external view returns (uint160 liquidityAmount);
189: function feeGrowth(uint256 strike, uint256 maturity) external view returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth);
197: function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees);
204:  function protocolFeesEarned(uint256 strike, uint256 maturity) external view returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees);
211: function totalLongBalance(uint256 strike, uint256 maturity) external view returns (uint256 long0Amount, uint256 long1Amount);
218:  function totalLongBalanceAdjustFees(uint256 strike, uint256 maturity) external view returns (uint256 long0Amount, uint256 long1Amount);
226: function totalPositions(uint256 strike, uint256 maturity) external view returns (uint256 longAmount, uint256 shortAmount);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L167
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L174
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L181
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L189
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L197
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L204
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L211
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L218
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L226

v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol
17: function parameter() external view returns (address poolFactory, address optionPair, uint256 transactionFee, uint256 protocolFee);
}
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol#L17

v2-pool/src/interfaces/ITimeswapV2PoolFactory
29: function get(address option) external view returns (address poolPair);
31: function getByIndex(uint256 id) external view returns (address optionPair);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L29
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L31

v2-pool/src/structs/Pool.sol
66:  function feeGrowth(Pool storage pool) external view returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {
75: function feesEarnedOf(Pool storage pool, address owner) external view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
83: function protocolFeesEarned(Pool storage pool) external view returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {
91: function totalLongBalanceAdjustFees(Pool storage pool, uint256 transactionFee) external view returns (uint256 long0Amount, uint256 long1Amount) {
101: function totalPositions(Pool storage pool, uint256 maturity, uint96 blockTimestamp) external view returns (uint256 longAmount, uint256 shortAmount) {
120: ) private pure returns (uint96 newLastTimestamp, uint256 newShortFeeGrowth) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L66
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L75
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L83
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L91
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L101
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L120

v2-pool/src/libraries/ConstantProduct.sol
51: function calculateGivenLiquidityDelta(uint160 rate, uint160 deltaLiquidity, uint96 duration, bool isAdd) internal pure returns (uint256 longAmount, uint256 shortAmount) {
66: function calculateGivenLiquidityLong(uint160 rate, uint256 longAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 shortAmount) {
81:  function calculateGivenLiquidityShort(uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 longAmount) {
103: ) internal pure returns (uint160 liquidityAmount, uint256 longAmount, uint256 shortAmount) {
141:  ) internal pure returns (uint160 newRate, uint256 longAmount, uint256 shortAmount, uint256 fees) {
171: ) internal pure returns (uint160 newRate, uint256 shortAmount, uint256 fees) {
202: ) internal pure returns (uint160 newRate, uint256 longAmount, uint256 fees) {
237: ) internal pure returns (uint160 newRate, uint256 longAmount, uint256 shortAmount, uint256 fees) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L51
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L66
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L81
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L103
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L141
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L171
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L202
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L237

v2-pool/src/libraries/FeeCalculation.sol
27: function update(uint160 liquidity, uint256 feeGrowth, uint256 protocolFees, uint256 fees, uint256 protocolFee) internal pure returns (uint256 newFeeGrowth, uint256 newProtocolFees) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/FeeCalculation.sol#L27

v2-pool/src/libraries/PoolFactory.sol
29: function get(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {
43: function getWithCheck(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L29
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L43

v2-pool/src/libraries/ConstantSum.sol
19:  function calculateGivenLongIn(uint256 strike, uint256 longAmountIn, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountOut, uint256 longFees) {
30:  function calculateGivenLongOut(uint256 strike, uint256 longAmountOut, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountIn, uint256 longFees) {
39: function calculateGivenLongOutAlreadyAdjustFees(uint256 strike, uint256 longAmountOut, bool isLong0ToLong1) internal pure returns (uint256 longAmountIn) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantSum.sol#L19
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantSum.sol#L30
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantSum.sol#L39

v2-pool/src/libraries/DurationCalculation.sol
18: function unsafeDurationFromLastTimestampToNow(uint96 lastTimestamp, uint96 blockTimestamp) internal pure returns (uint96 duration) {
26: function unsafeDurationFromNowToMaturity(uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {
34: function unsafeDurationFromLastTimestampToMaturity(uint96 lastTimestamp, uint256 maturity) internal pure returns (uint96 duration) {
43: function unsafeDurationFromLastTimestampToNowOrMaturity(uint96 lastTimestamp, uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L18
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L26
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L34
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L43

v2-pool/src/libraries/DurationWeight.sol
15: function update(uint160 liquidity, uint256 shortFeeGrowth, uint256 shortAmount) internal pure returns (uint256 newShortFeeGrowth) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationWeight.sol#L15

v2-token/src/interfaces/ITimeswapV2Token.sol
18: function positionOf(address owner, TimeswapV2TokenPosition calldata position) external view returns (uint256 amount);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L18

v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol
26:  function positionOf(address owner, TimeswapV2LiquidityTokenPosition calldata position) external view returns (uint256 amount);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L26

v2-token/src/TimeswapV2LiquidityToken.sol
65: function positionOf(address owner, TimeswapV2LiquidityTokenPosition calldata timeswapV2LiquidityTokenPosition) external view returns (uint256 amount) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L65

v2-token/src/TimeswapV2Token.sol
66: function positionOf(address owner, TimeswapV2TokenPosition calldata timeswapV2TokenPosition) public view returns (uint256 amount) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L66

v2-token/src/structs/FeesPosition.sol
23: ) internal pure returns (uint256 long0Fee, uint256 long1Fee, uint256 shortFee) {
46: ) internal view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L23
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L46

v2-option/src/interfaces/ITimeswapV2Option.sol
120:  function getByIndex(uint256 id) external view returns (StrikeAndMaturity memory);
130: function totalPosition(uint256 strike, uint256 maturity, TimeswapV2OptionPosition position) external view returns (uint256 balance);
138: function positionOf(uint256 strike, uint256 maturity, address owner, TimeswapV2OptionPosition position) external view returns (uint256 balance);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L120
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L130
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L138

v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol
16:  function parameter() external view returns (address optionFactory, address token0, address token1);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol#L16

v2-option/src/interfaces/ITimeswapV2OptionFactory.sol
24:  function get(address token0, address token1) external view returns (address optionPair);
28: function getByIndex(uint256 id) external view returns (address optionPair);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L24
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L28

v2-option/src/structs/Option.sol
38: function totalPosition(Option storage option, uint256 strike, TimeswapV2OptionPosition position) internal view returns (uint256 balance) {
49: function positionOf(Option storage option, address owner, TimeswapV2OptionPosition position) internal view returns (uint256 balance) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L38
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L49

v2-option/src/TimeswapV2OptionFactory.sol
30: function get(address token0, address token1) external view override returns (address optionPair) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2OptionFactory.sol#L30

v2-option/src/libraries/OptionFactory.sol
27: function get(address optionFactory, address token0, address token1) internal view returns (address optionPair) {
37:  function getWithCheck(address optionFactory, address token0, address token1) internal view returns (address optionPair) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/libraries/OptionFactory.sol#L27
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/libraries/OptionFactory.sol#L37

[G-02] <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES
Proof Of Concept
v2-library/src/FullMath.sol
163: productA1 += (quotient1 * divisor);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L163

v2-pool/src/structs/LiquidityPosition.sol
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
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L55
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L56
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L57
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L66
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L70
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L71
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L72
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L76
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L80
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L81
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L82

v2-pool/src/structs/Pool.sol#L361
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
719: pool.long0Balance -= (long0Amount + longFees);
722: pool.long1Balance += long1Amount;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L361
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L362
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L365
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L452
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L453
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L455
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L537
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L538
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L636
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L650
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L677
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L689
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L695
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L710
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L719
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L722

v2-pool/src/libraries/ConstantProduct.sol
149: if (isAdd) longAmount -= fees;
150: else shortAmount -= fees;
180: shortAmount -= fees;
212: longAmount -= fees;
244: amount -= fees;
295: denominator2 += longAmount;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L149
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L150
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L180
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L212
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L244
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L295

v2-token/src/base/ERC1155Enumerable.sol
61: _idTotalSupply[id] += amount;
95: _idTotalSupply[id] -= amount;
117: _currentIndex[to] += 1;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L61
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L95
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L117

v2-token/src/structs/FeesPosition.sol
31: feesPosition.long0Fees += FeeCalculation.getFees(liquidity, feesPosition.long0FeeGrowth, long0FeeGrowth);
32: feesPosition.long1Fees += FeeCalculation.getFees(liquidity, feesPosition.long1FeeGrowth, long1FeeGrowth);
33: feesPosition.shortFees += FeeCalculation.getFees(liquidity, feesPosition.shortFeeGrowth, shortFeeGrowth);
53: feesPosition.long0Fees += long0Fees;
54: feesPosition.long1Fees += long1Fees;
55: feesPosition.shortFees += shortFees;
59: feesPosition.long0Fees -= long0Fees;
60: feesPosition.long1Fees -= long1Fees;
61: feesPosition.shortFees -= shortFees;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L31
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L32
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L33
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L53
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L54
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L55
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L59
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L60
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L61

v2-option/src/structs/Option.sol
62: option.long0[msg.sender] -= amount;
63: option.long0[to] += amount;
65: option.long1[to] += amount;
66: option.long1[msg.sender] -= amount;
68: option.short[msg.sender] -= amount;
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
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L62
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L63
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L65
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L66
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L68
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L69
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L103
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L104
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L108
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L109
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L112
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L141
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L142
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L173
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L174
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L175
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L177
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L178
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L179
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L208
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L209

v2-option/src/TimeswapV2Option.sol
190: option.long0[msg.sender] -= token0AndLong0Amount;
191: option.long1[msg.sender] -= token1AndLong1Amount;
192: option.short[msg.sender] -= shortAmount;
237: if (param.isLong0ToLong1) option.long0[msg.sender] -= token0AndLong0Amount;
238: else option.long1[msg.sender] -= token1AndLong1Amount;
277: option.short[msg.sender] -= shortAmount;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L190
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L191
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L192
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L237
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L238
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L277


[G-03]  Struct packing
We can use the uint types uint8, uint16, uint32, etc, though Solidity reserves 256 bits of storage regardless of the uint type meaning, in normal instances, there's no cost saving. However, if you have multiple uints inside a struct, using a smaller-sized uint when possible will allow Solidity to pack these variables together to take up less storage. 
Proof Of Concept
v2-pool/src/structs/CallbackParam.sol:
16: uint160 liquidityAmount;
34: uint160 liquidityAmount;
54: uint160 liquidityAmount;
72: uint160 liquidityAmount;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/CallbackParam.sol#L16
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/CallbackParam.sol#L34
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/CallbackParam.sol#L54
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/CallbackParam.sol#L72

v2-pool/src/structs/Pool.sol#L119
43: uint96 lastTimestamp;
118: uint96 duration,
119: uint96 blockTimestamp
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L43
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L118
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L119

v2-token/src/structs/CallbackParam.sol
55: uint160 liquidityAmount;
70:  uint160 liquidityAmount;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/CallbackParam.sol#L55
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/CallbackParam.sol#L70

v2-token/src/structs/Param.sol
72:  uint160 liquidityAmount;
90: uint160 liquidityAmount;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L72
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L90

[G-04] Use assembly to check for address(0)
Proof Of Concept
v2-token/src/base/ERC1155Enumerable.sol
59:  if (from == address(0)) {
64: if (to != address(0) && to != from) {
93: if (to == address(0)) {
98: if (from != address(0) && from != to) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L59
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L64
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L93
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L98

v2-token/src/TimeswapV2LiquidityToken.sol
76: if (from == address(0)) Error.zeroAddress();
77: if (to == address(0)) Error.zeroAddress();
249: if (from != address(0)) _feesPositions[id][from].update(uint160(balanceOf(from, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
251: if (to != address(0)) _feesPositions[id][to].update(uint160(balanceOf(to, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L76
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L77
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L249
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L251

v2-token/src/structs/Param.sol
119: if (param.long0To == address(0) || param.long1To == address(0) || param.shortTo == address(0)) Error.zeroAddress();
126: if (param.long0To == address(0) || param.long1To == address(0) || param.shortTo == address(0)) Error.zeroAddress();
133:  if (param.to == address(0)) Error.zeroAddress();
140: if (param.to == address(0)) Error.zeroAddress();
147:  if (param.to == address(0)) Error.zeroAddress();
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L119
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L126
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L133
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L140
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L147

[G-05] STORAGE POINTER TO A STRUCTURE IS CHEAPER THAN COPYING EACH VALUE OF THE STRUCTURE INTO MEMORY, SAME FOR ARRAY AND MAPPING
It may not be obvious, but every time you copy a storage struct/array/mapping to a memory variable, you are literally copying each member by reading it from storage, which is expensive. And when you use the storage keyword, you are just storing a pointer to the storage, which is much cheaper.
Proof Of Concept
v2-token/src/TimeswapV2LiquidityToken.sol
102: TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition = TimeswapV2LiquidityTokenPosition({
238: TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition = _timeswapV2LiquidityTokenPositions[id];
264: TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition = _timeswapV2LiquidityTokenPositions[id];
275: FeesPosition memory feesPosition = _feesPositions[id][owner];
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L102
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L238
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L264
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L275

v2-token/src/TimeswapV2Token.sol
89:  TimeswapV2TokenPosition memory timeswapV2TokenPosition = TimeswapV2TokenPosition({
119:  TimeswapV2TokenPosition memory timeswapV2TokenPosition = TimeswapV2TokenPosition({
148: TimeswapV2TokenPosition memory timeswapV2TokenPosition = TimeswapV2TokenPosition({
231: TimeswapV2TokenPosition memory timeswapV2TokenPosition = TimeswapV2TokenPosition({
245: TimeswapV2TokenPosition memory timeswapV2TokenPosition = TimeswapV2TokenPosition({
259: TimeswapV2TokenPosition memory timeswapV2TokenPosition = TimeswapV2TokenPosition({
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L89
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L119
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L148
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L231
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L245
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L259

[G-06] SETTING THE CONSTRUCTOR TO PAYABLE
Saves ~13 gas per instance
Proof Of Concept
19:constructor() {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/NoDelegateCall.sol#L19
18: constructor(address chosenOwner) {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L18
41:  constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L41
19: constructor() {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/NoDelegateCall.sol#L19
65: constructor() NoDelegateCall() {
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L65

[G-07] MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN ADDRESS/ID TO A STRUCT, WHERE APPROPRIATE
If both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.
Proof Of Concept
v2-pool/src/TimeswapV2Pool.sol
52: mapping(uint256 => mapping(uint256 => uint96)) private reentrancyGuards;
53: mapping(uint256 => mapping(uint256 => Pool)) private pools;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L52
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L53

v2-pool/src/TimeswapV2PoolFactory.sol
31:  mapping(address => address) private pairs;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L31

v2-token/src/base/ERC1155Enumerable.sol
15: mapping(address => mapping(uint256 => uint256)) private _ownedTokens; // An index of all tokens
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L15

v2-token/src/TimeswapV2LiquidityToken.sol
47: mapping(uint256 => mapping(address => FeesPosition)) private _feesPositions;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L47

v2-option/src/structs/Option.sol
23: mapping(address => uint256) long0;
24: mapping(address => uint256) long1;
25: mapping(address => uint256) short;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L23
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L24
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L25

v2-option/src/TimeswapV2OptionFactory.sol
22:  mapping(address => mapping(address => address)) private optionPairs;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2OptionFactory.sol#L22

v2-option/src/TimeswapV2Option.sol
48: mapping(uint256 => mapping(uint256 => Option)) private options;
53: mapping(uint256 => mapping(uint256 => bool)) private hasInteracted;
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L48
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L53

