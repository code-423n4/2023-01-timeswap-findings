## 1. Incorrect NatSpec

Misleading NatSpec could potentially lead to confusion from readers.

In the following NatSpec, `@param` is being used incorrectly:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol#L11-L17

```solidity
11    /// @notice Get the parameters to be used in constructing the pair, set transiently during pair creation.
12    /// @dev Called by the pair constructor to fetch the parameters of the pair.
13    /// @return poolFactory The poolFactory address.

      /// @audit "@param" is incorrect as the below is not a parameter.
14    /// @param optionPair The Timeswap V2 OptionPair address.

      /// @audit "@param" is incorrect as the below is not a parameter.
15    /// @param transactionFee The transaction fee earned by the liquidity providers.

      /// @audit "@param" is incorrect as the below is not a parameter.
16    /// @param protocolFee The protocol fee earned by the DAO.
17    function parameter() external view returns (address poolFactory, address optionPair, uint256 transactionFee, uint256 protocolFee);
```

## 2. Limit line length

To comply with the [Solidity Style Guide](https://docs.soliditylang.org/en/develop/style-guide.html#maximum-line-length), lines should be limited to 120 characters max.

As an example, here are some instances of this issue:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L46

```solidity
46    function add512(uint256 addendA0, uint256 addendA1, uint256 addendB0, uint256 addendB1) internal pure returns (uint256 sum0, uint256 sum1) {

62    function sub512(uint256 minuend0, uint256 minuend1, uint256 subtrahend0, uint256 subtrahend1) internal pure returns (uint256 difference0, uint256 difference1) {

158    function div512(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient0, uint256 quotient1) {

287    function sqrt512Estimate(uint256 value0, uint256 value1, uint256 currentEstimate) private pure returns (uint256 estimate) {
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L6

```solidity
6: import {TimeswapV2PoolCollectParam, TimeswapV2PoolMintParam, TimeswapV2PoolBurnParam, TimeswapV2PoolDeleverageParam, TimeswapV2PoolLeverageParam, TimeswapV2PoolRebalanceParam} from "../structs/Param.sol";

81:    event Mint(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);

115:    event Deleverage(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);

126:    event Leverage(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address long0To, address long1To, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);
```

## 3. Use `delete` to clear variables instead of zero assignment, i.e. (0, 0x0, false)

A better way to indicate that you are clearing a variable is to use the [`delete`](https://docs.soliditylang.org/en/v0.8.17/types.html#delete) keyword.

Here are a few examples of this issue:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L166

```solidity
166:                    quotient0 = 0;
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L228

```solidity
228            pool.long0ProtocolFees = 0;

236            pool.long1ProtocolFees = 0;

244            pool.shortProtocolFees = 0;

633                pool.long0Balance = 0;

646                pool.long1Balance = 0;

685                    pool.long1Balance = 0;
```

## 4. Zero value checks not fully implemented

Zero value checks are in general a best practice. However, the following functions are missing zero checks for both `strike` and `maturity`:

File: `TimeswapV2Pool.sol` [Line 175-181](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L175-L181)

```solidity
    function initialize(uint256 strike, uint256 maturity, uint160 rate) external override noDelegateCall {
        if (maturity < blockTimestamp(0)) Error.alreadyMatured(maturity, blockTimestamp(0));
        /// @audit `strike` and `maturity` should also be checked for zero.
        if (rate == 0) Error.cannotBeZero();
        addPoolEnumerationIfNecessary(strike, maturity);

        pools[strike][maturity].initialize(rate);
    }
```

The same checks above should also be implemented in `transferFees()`:

File: `TimeswapV2Pool.sol` [Line 165-172](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L165-L172)

```solidity
    function transferFees(uint256 strike, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external override {
        if (to == address(0)) Error.zeroAddress();
        if (long0Fees == 0 && long1Fees == 0 && shortFees == 0) Error.zeroInput();

        pools[strike][maturity].transferFees(maturity, to, long0Fees, long1Fees, shortFees, blockTimestamp(0));

        emit TransferFees(strike, maturity, msg.sender, to, long0Fees, long1Fees, shortFees);
    }
```

## 5. Typos

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L81

```solidity
       /// @audit overidden
81:    // Can be overidden for testing purposes.

        /// @audit "receipients" and "receipient"
222:    /// @dev Transfer long0 positions, long1 positions, and/or short positions to the receipients.
...
225:    /// @param long0To The receipient of long0 positions.
226:    /// @param long1To The receipient of long1 positions.
227:    /// @param shortTo The receipient of short positions.
...
332:        // Transfer the positions to the receipients.
...
399:        // Transfer short positions to the receipient.
446:        // Transfer the positions to the receipients.
...
486:        // Transfer the positions to the receipients.
```

## 6. Unused Named Return

To improve code clarity, consider either utilizing the following unused named return or removing it:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L88-L90

```solidity
    /// @audit `uint256 result`
    function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {
        return value1 < value2 ? value1 : value2;
    }
```

## 7. Throwing an incorrect error may cause confusion

Errors should accurately reflect the failing condition. However, in `TimeswapV2PoolFactory.sol`, the error that is thrown at [line 63](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63) is misleading:

File: `TimeswapV2PoolFactory.sol` [Line 59-70](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L59-L70)

```solidity
59    function create(address optionPair) external override returns (address pair) {
60        if (optionPair == address(0)) Error.zeroAddress();
61
62        pair = pairs[optionPair];

          /// @audit `Error.zeroAddress()` should not be thrown in the if statement below.
63        if (pair != address(0)) Error.zeroAddress();
64
65        pair = deploy(address(this), optionPair, transactionFee, protocolFee);
66
67        pairs[optionPair] = pair;
68
69        emit Create(msg.sender, optionPair, pair);
70    }
```
