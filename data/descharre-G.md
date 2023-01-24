# Summary
|ID     | Finding|  Gas saved| Instances |
|:----: | :---           |              :----:    |  :----:         |
|1       | Unneccesary checks | - | 1 |
|2       | Add unchecked to increments or decrements that can't under/overflow | 217 | 7 |
|3       | Redundant checks | - | 6 |
|4       | Call block.timestamp directly in an if statement | - | 3 |
|5       | Use double if statements instead of && | 60 | 7 |
|6       | Remove return variable when it's never accessed | 30 | 1 |

# Details
## 1 Unneccesary checks
Check if an enum exist is unneccesary, EVM will revert if you try to input an enum value does not exist. Saves some gas because you don't need the check.

[TimeswapV2Option.sol#L101](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L101)
```solidity
    PositionLibrary.check(position);
```
[Position.sol#L22-L24](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Position.sol#L22-L24)
```solidity
    function check(TimeswapV2OptionPosition position) internal pure {
        if (uint256(position) >= 3) revert InvalidPosition();
    }
```
## 2 Add unchecked to increments or decrements that can't under/overflow
If amount is really large, the evm will do an underflow check on the decrement of msg.sender and then revert. So amount can never be large enough to overflow when you increment the to address.

[Option.sol#L60-L71](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L60-L71)
```diff
    function transferPosition(Option storage option, address to, TimeswapV2OptionPosition position, uint256 amount) internal {
        if (position == TimeswapV2OptionPosition.Long0) {
            option.long0[msg.sender] -= amount;
+           unchecked{
            option.long0[to] += amount;
+           }
        } else if (position == TimeswapV2OptionPosition.Long1) {
-           option.long1[to] += amount;
            option.long1[msg.sender] -= amount;
+           unchecked{
+           option.long1[to] += amount;
+           }
        } else if (position == TimeswapV2OptionPosition.Short) {
            option.short[msg.sender] -= amount;
+           unchecked{
            option.short[to] += amount;
+           }
        }
    }
```

Same applies to:
- [Option.sol#L175](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L175): if amount is too large, increment above will revert because totalLong is always more than long of 1 user.
- [Option.sol#L179](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L179): if amount is too large, increment above will revert
- [TimeswapV2LiquidityToken.sol#L114](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L114): increments with 1 and id is a uint256 so there is no possibility of overflowing. TimeswapV2LiquidityToken `mint()` 34 gas saved.

Id increments always by 1. TimeswapV2Token `mint()` 183 gas saved
- [TimeswapV2Token.sol#L103](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L103)
- [TimeswapV2Token.sol#L133](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L133)
- [TimeswapV2Token.sol#L162](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L162)
## 3 Redundant checks
- [Pool.sol#L262-L282](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L262-L282):
```diff
+       LiquidityPosition storage liquidityPosition = pool.liquidityPositions[msg.sender];
        if (pool.liquidity != 0) {
            if (maturity > blockTimestamp) updateDurationWeightBeforeMaturity(pool, blockTimestamp);
            else if (pool.lastTimestamp < maturity) updateDurationWeightAfterMaturity(pool, maturity, blockTimestamp);
+           liquidityPosition.update(pool.long0FeeGrowth, pool.long1FeeGrowth, pool.shortFeeGrowth);
        }

        // Update the fee growth and fees of caller.
-       LiquidityPosition storage liquidityPosition = pool.liquidityPositions[msg.sender];

-       if (pool.liquidity != 0) liquidityPosition.update(pool.long0FeeGrowth, pool.long1FeeGrowth, pool.shortFeeGrowth);

        (long0Amount, long1Amount, shortAmount) = liquidityPosition.collectTransactionFees(long0Requested, long1Requested, shortRequested);
```
- [Pool.sol#L160](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L160): liquidity of the pool is already checked in the [`transferLiquidity()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L153) function in the TimeswapV2Pool contract.
- [Pool.sol#L385](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L385): liquidity of the pool is already checked in the [`burn()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L322) function in the TimeswapV2Pool contract.
- [Pool.sol#L476](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L476): liquidity of the pool is already checked in the [`deleverage()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L382) function in the TimeswapV2Pool contract.
- [Pool.sol#L559](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L559): liquidity of the pool is already checked in the [`leverage()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L435) function in the TimeswapV2Pool contract.
- [Pool.sol#L666](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L666): liquidity of the pool is already checked in the [`rebalance()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L470) function in the TimeswapV2Pool contract.


## 4 Call block.timestamp directly in an if statement
When you compare the block timestamp of the current block to something. It's cheaper to use block.timestamp directly than calling the blockTimeStamp function. In the following lines it's also not necessary to cast block.timestamp to uint96.

- [TimeswapV2Pool.sol#L155](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L155)
- [TimeswapV2Pool.sol#L176](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L176)

On other places like error handling it's also not necessary to cast block.timestamp to a uint96.

- [Error.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol#L50)
```solidity
    /// @dev Reverts when the maturity is already matured.
    /// @param maturity The maturity.
    /// @param blockTimestamp The current block timestamp.
    function alreadyMatured(uint256 maturity, uint96 blockTimestamp) internal pure {
        revert AlreadyMatured(maturity, blockTimestamp);
    }

    /// @dev Reverts when the maturity is still active.
    /// @param maturity The maturity.
    /// @param blockTimestamp The current block timestamp.
    function stillActive(uint256 maturity, uint96 blockTimestamp) internal pure {
        revert StillActive(maturity, blockTimestamp);
    }
```

## 5 Use double if statements instead of &&
Using multiple if statements instead of && is cheaper when the statement fails. However there will be more deployment costs but this will only be one time.

In the following example, if the first statement fails it will save about 60 gas, second statement fails will save about 40 gas. And if the last statement fails it will save 20 gas.
[TimeswapV2Pool.sol#L167](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L167)
```diff
-    if (long0Fees == 0 && long1Fees == 0 && shortFees == 0) Error.zeroInput();
+    if (long0Fees == 0) Error.zeroInput();
+    if (long1Fees == 0) Error.zeroInput();
+    if (shortFees == 0) Error.zeroInput();
```
Gas optimization can also be done at:
- [v2-pool/Param.sol#L136](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L136)
- [v2-option/Param.sol#L111](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L111)
- [v2-option/Param.sol#L124](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L124)
- [v2-token/Param.sol#L121](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L121)
- [v2-token/Param.sol#L128](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L128)
- [v2-token/Param.sol#L149](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L149)

## 6 Remove return variable when it's never accessed
Return variable optionPair is never accessed when getWithCheck function is called. Removing the return variables saves around 15 gas for `burn()` and `mint()`.

[PoolFactory.sol#L43-L49](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L43-L49)
```diff
-   function getWithCheck(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {
+   function getWithCheck(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address poolPair) {
-       optionPair = optionFactory.getWithCheck(token0, token1);
+       address optionPair = optionFactory.getWithCheck(token0, token1);
        poolPair = ITimeswapV2PoolFactory(poolFactory).get(optionPair);

        if (poolPair == address(0)) Error.zeroAddress();
    }
```
[TimeswapV2LiquidityToken.sol#L157](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L157)
```diff
-       (, address poolPair) = PoolFactoryLibrary.getWithCheck(optionFactory, poolFactory, param.token0, param.token1);
+       (address poolPair) = PoolFactoryLibrary.getWithCheck(optionFactory, poolFactory, param.token0, param.token1);
```

Is also called at 
- [TimeswapV2LiquidityToken.sol#L190](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L190)
- [TimeswapV2LiquidityToken.sol#L244](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L244)
- [TimeswapV2LiquidityToken.sol#L270](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L270)