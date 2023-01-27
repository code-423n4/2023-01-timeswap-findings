## Gas Optimizations

| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW | 8 |
| [GAS-2](#GAS-2) | X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES | 72 |
| [GAS-3](#GAS-3) | NOT USING THE NAMED RETURN VARIABLES WHEN A FUNCTION RETURNS, WASTES DEPLOYMENT GAS | 16 |
| [GAS-4](#GAS-4) | USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD | 9 |
| [GAS-5](#GAS-5) |  USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS | 12 |

### [GAS-1] ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop.

*Instances (8):*
```solidity
File: v2-library/src/FullMath.sol

146:      if (dividend1 > productA1 || dividend0 > productA0) quotient++;

167:      quotient1++;
168:      } else quotient0++;

257:      if (roundUp && mulmod(multiplicand, multiplier, divisor) != 0) result++;

277:      if (value1 > product1 || value0 > product0) result++;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol)

```solidity
File: v2-library/src/Math.sol

51:      if (roundUp && dividend % divisor != 0) quotient++;

62:      if (roundUp && dividend % (1 << divisorBit) != 0) quotient++;

81:      if (roundUp && value % result != 0) result++;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol)

###  [GAS-2] X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

*Instances (72)*:
```solidity
File: v2-library/src/FullMath.sol

163:      productA1 += (quotient1 * divisor);

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L163)

```solidity
File: v2-option/src/structs/Option.sol

62:      option.long0[msg.sender] -= amount;
63:      option.long0[to] += amount;

65:      option.long1[to] += amount;
66:      option.long1[msg.sender] -= amount;

68:      option.short[msg.sender] -= amount;
69:      option.short[to] += amount;

103:     option.totalLong0 += token0AndLong0Amount;
104:     option.long0[long0To] += token0AndLong0Amount;

108:     option.totalLong1 += token1AndLong1Amount;
109:     option.long1[long1To] += token1AndLong1Amount;

112:     option.short[shortTo] += shortAmount;

141:     option.totalLong0 -= token0AndLong0Amount;
142:     option.totalLong1 -= token1AndLong1Amount;

173:     option.totalLong0 -= token0AndLong0Amount;
174:     option.totalLong1 += token1AndLong1Amount;
175:     option.long1[longTo] += token1AndLong1Amount;

177:     option.totalLong1 -= token1AndLong1Amount;
178:     option.totalLong0 += token0AndLong0Amount;
179:     option.long0[longTo] += token0AndLong0Amount;

208:     option.totalLong0 -= token0Amount;
209:     option.totalLong1 -= token1Amount;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol)

```solidity
File: v2-pool/src/libraries/ConstantProduct.sol

149:      if (isAdd) longAmount -= fees;
150:      else shortAmount -= fees;

180:      shortAmount -= fees;

212:      longAmount -= fees;

244:      amount -= fees;

295:      denominator2 += longAmount;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L295)

```solidity
File: v2-pool/src/structs/LiquidityPosition.sol

55:      liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth);
56:      liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth);
57:      liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth);

66:      liquidityPosition.liquidity += liquidityAmount;

70:      liquidityPosition.long0Fees += long0Fees;
71:      liquidityPosition.long1Fees += long1Fees;
72:      liquidityPosition.shortFees += shortFees;

76:      liquidityPosition.liquidity -= liquidityAmount;

80:      liquidityPosition.long0Fees -= long0Fees;
81:      liquidityPosition.long1Fees -= long1Fees;
82:      liquidityPosition.shortFees -= shortFees;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol)

```solidity
File: v2-pool/src/structs/Pool.sol

361:      if (long0Amount != 0) pool.long0Balance += long0Amount;
362:      if (long1Amount != 0) pool.long1Balance += long1Amount;

365:      pool.liquidity += liquidityAmount;

452:      if (long0Amount != 0) pool.long0Balance -= long0Amount;
453:      if (long1Amount != 0) pool.long1Balance -= long1Amount;

455:      pool.liquidity -= liquidityAmount;

537:      if (long0Amount != 0) pool.long0Balance += long0Amount;
538:      if (long1Amount != 0) pool.long1Balance += long1Amount;

636:      pool.long0Balance -= (long0Amount + long0Fees);

650:      pool.long1Balance -= (long1Amount + long1Fees);

677:      pool.long1Balance -= (long1Amount + longFees);

689:      pool.long1Balance -= (long1Amount + longFees);

695:      pool.long0Balance += long0Amount;

710:      pool.long0Balance -= (long0Amount + longFees);

719:      pool.long0Balance -= (long0Amount + longFees);

722:      pool.long1Balance += long1Amount;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol)

```solidity
File: v2-token/src/base/ERC1155Enumerable.sol

61:      _idTotalSupply[id] += amount;

95:      _idTotalSupply[id] -= amount;

117:     _currentIndex[to] += 1;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol)

```solidity
File: v2-token/src/structs/FeesPosition.sol

31:      feesPosition.long0Fees += FeeCalculation.getFees(liquidity, feesPosition.long0FeeGrowth, long0FeeGrowth);
32:      feesPosition.long1Fees += FeeCalculation.getFees(liquidity, feesPosition.long1FeeGrowth, long1FeeGrowth);
33:      feesPosition.shortFees += FeeCalculation.getFees(liquidity, feesPosition.shortFeeGrowth, shortFeeGrowth);  

53:      feesPosition.long0Fees += long0Fees;
54:      feesPosition.long1Fees += long1Fees;
55:      feesPosition.shortFees += shortFees;

59:      feesPosition.long0Fees -= long0Fees;
60:      feesPosition.long1Fees -= long1Fees;
61:      feesPosition.shortFees -= shortFees;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol)

```solidity
File: v2-option/src/TimeswapV2Option.sol

190:     option.long0[msg.sender] -= token0AndLong0Amount;
191:     option.long1[msg.sender] -= token1AndLong1Amount;
192:     option.short[msg.sender] -= shortAmount;

237:     if (param.isLong0ToLong1) option.long0[msg.sender] -= token0AndLong0Amount;
238:     else option.long1[msg.sender] -= token1AndLong1Amount;

277:     option.short[msg.sender] -= shortAmount;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol)

### [GAS-3] NOT USING THE NAMED RETURN VARIABLES WHEN A FUNCTION RETURNS, WASTES DEPLOYMENT GAS

*Instances (16):*
```solidity
File: v2-library/src/Math.sol

69:      function sqrt(uint256 value, bool roundUp) internal pure returns (uint256 result) {

88:      function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol)

```solidity
File: v2-pool/src/TimeswapV2Pool.sol

118:      function feeGrowth(uint256 strike, uint256 maturity) external view override returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {

123:      function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view override returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

128:      function protocolFeesEarned(uint256 strike, uint256 maturity) external view override returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {

240:      function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

248:      ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

305:      function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

313:      ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

365:      function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

373:      ) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

421:      function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

426:      function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol)

```solidity
File: v2-pool/src/structs/Pool.sol

66:      function feeGrowth(Pool storage pool) external view returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {

75:      function feesEarnedOf(Pool storage pool, address owner) external view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

83:      function protocolFeesEarned(Pool storage pool) external view returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol)

### [GAS-4] USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD

*Instances (9):*
```solidity
File: v2-pool/src/structs/CallbackParam.sol

16:    uint160 liquidityAmount;

34:    uint160 liquidityAmount;

54:    uint160 liquidityAmount;

72:    uint160 liquidityAmount;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol)

```solidity
File: v2-token/src/structs/CallbackParam.sol

55:    uint160 liquidityAmount;

70:    uint160 liquidityAmount;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L16)

```solidity
File: v2-pool/src/structs/Param.sol

72:    uint160 liquidityAmount;

90:    uint160 liquidityAmount;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L16)

```solidity
File: v2-pool/src/structs/LiquidityPosition.sol

16:    uint160 liquidity;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L16)

### [GAS-5] USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS

*Instances (12):*
```solidity
File: v2-option/src/TimeswapV2Option.sol

41:    address public immutable override optionFactory;

43;    address public immutable override token0;

45:    address public immutable override token1;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol)

```solidity
File: v2-option/src/TimeswapV2Pool.sol

44:    address public immutable override poolFactory;

46:    address public immutable override optionPair;

48:    uint256 public immutable override transactionFee;

50:    uint256 public immutable override protocolFee;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Pool.sol)

```solidity
File: v2-option/src/TimeswapV2PoolFactory.sol

27:    uint256 public immutable override transactionFee;

29:    uint256 public immutable override protocolFee;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2PoolFactory.sol)

```solidity
File: v2-token/src/TimeswapV2LiquidityToken.sol

33:    address public immutable optionFactory;
34:    address public immutable poolFactory;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol)

```solidity
File: v2-token/src/TimeswapV2Token.sol

34:    address public immutable optionFactory;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol)