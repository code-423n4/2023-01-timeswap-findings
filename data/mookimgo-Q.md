# 1. forge-std dependency should be removed from TimeswapV2Token

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L109

`console.log` should be removed before deploying.

# 2. changeInteractedIfNecessary, raiseGuard, and lowerGuard should be moved to a parent contract

Both TimeswapV2Token and TimeswapV2LiquidityToken use these three functions, consider moving them to a shred parent contract.

# 3. misleading docs of ReentrancyGuard

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L21

@notice Reverts when balanceTarget is not zero.

but there's no balanceTarget, this comment should be removed.

# 4. misleading numberOfPairs function in TimeswapV2OptionFactory

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L36

`numberOfPairs` function return length of getByIndex, but the `getByIndex` list is not modified in create.

Remove this numberOfPairs function and getByIndex variable, or add a write `getByIndex.push(optionPair)` in create.