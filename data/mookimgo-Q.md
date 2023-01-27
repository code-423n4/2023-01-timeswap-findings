# 1. forge-std dependency should be removed from TimeswapV2Token

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L109

`console.log` should be removed before deploying.

# 2. changeInteractedIfNecessary, raiseGuard, and lowerGuard should be moved to a parent contract

Both TimeswapV2Token and TimeswapV2LiquidityToken use these three functions, consider moving them to a shred parent contract.