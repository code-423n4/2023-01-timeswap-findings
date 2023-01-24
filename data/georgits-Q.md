## Remove unused imports
Option.sol - [5](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L5), [10](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L10)(PositionLibrary), [11](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L11)(TransactionLibrary)

TimeswapV2Option.sol - [24](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L24)

Pool.sol - [8](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L8), [25](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L25)(TransactionLibrary), [27](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L27)(TimeswapV2PoolCollectParam)

TimeswapV2Pool.sol - [21](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L21)(ITimeswapV2PoolBurnCallback), [32](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L32)(TimeswapV2PoolMintChoiceCallbackParam, TimeswapV2PoolBurnChoiceCallbackParam, TimeswapV2PoolDeleverageChoiceCallbackParam, TimeswapV2PoolLeverageChoiceCallbackParam), [34](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L34)(TransactionLibrary)

## Wrong NatSpec `@dev` description
Option.sol - [145](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L145), [183](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L183)

## Use latest Solidity version with a stable pragma statement
[ERC1155Enumerable.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol), [IERC1155Enumerable.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol), [CallbackParam.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol), [FeesPosition.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol), [Param.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol), [Position.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol), [TimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol), [TimeswapV2Token.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol)

## Code reverts with wrong error
TimeswapV2PoolFactory.sol - [63](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63)
