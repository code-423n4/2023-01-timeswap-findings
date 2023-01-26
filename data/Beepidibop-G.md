# Gas Findings

## [GAS-1] `ReentrancyGuard`: Consider deprecating `NOT_INTERACTED` and `changeInteractedIfNecessary`

Consider deprecating checking for `NOT_INTERACTED` in `ReentrancyGuard.check` since actions such as `mint`/`burn` that require `ReentrancyGuard.check()` will revert due to division by zero or underflows and collecting fees would collect `0` if nothing was minted in the first place.

Places that use `ReentrancyGuard.check` and what stops it from being necessary

| Contract                   | Function                   | Reason                                                                                                                                                                                                             |
| -------------------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `TimeswapV2Pool`           | `collectProtocolFees()`    | Would collect 0 fees                                                                                                                                                                                               |
| `TimeswapV2Pool`           | `collectTransactionFees()` | Would collect 0 fees                                                                                                                                                                                               |
| `TimeswapV2Pool`           | `mint()`                   | Reverts on divide by 0 [here](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L306) (`pool.sqrtInterestRate` is `0`)            |
| `TimeswapV2Pool`           | `burn()`                   | Reverts on `hasLiquidity()` |
| `TimeswapV2Pool`           | `deleverage()`             | Reverts on `hasLiquidity()`                                                                                                                                                                                        |
| `TimeswapV2Pool`           | `leverage()`               | Reverts on `hasLiquidity()`                                                                                                                                                                                        |
| `TimeswapV2Pool`           | `rebalance()`              | Reverts on `hasLiquidity()`                                                                                                                                                                                        |
| `TimeswapV2LiquidityToken` | `mint()`                   | `changeInteractedIfNecessary()` in the line right before, which means minting without needing interaction is intended behaviour                                                                                    |
| `TimeswapV2LiquidityToken` | `burn()`                   | Reverts on underflow                                                                                                                                                                                               |
| `TimeswapV2LiquidityToken` | `collect()`                | Transfers `0` fees even if `collect()` is properly implemented (_[M-1] `TimeswapV2LiquidityToken`: `collect()` will always revert because it uses the wrong paramenters when calling `ITimeswapV2Pool.transferFees()`_) |
| `TimeswapV2Token`          | `mint()`                   | `changeInteractedIfNecessary()` in the line right before, which means minting without needing interaction is intended behaviour                                                                                    |
| `TimeswapV2Token`          | `burn()`                   | Reverts on underflow                                                                                                                                                                                               |

### Recommendation

Consider deprecating checking for `NOT_INTERACTED` in `ReentrancyGuard.check` and `changeInteractedIfNecessary`.

### Link To Affected Code

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L24

## [GAS-2] `PoolLibrary`: Consider putting `pool.xProtocolFees` into memory before comparison in `collectProtocolFees()`

Putting `pool.xProtocolFees` in memory before the `if` statement for the 3 types of fees can save gas on `SLOAD`ing the same slot multiple times.

### Recommendation

Consider putting `pool.xProtocolFees` into memory before comparison in `collectProtocolFees()`

### Link To Affected Code

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L226
