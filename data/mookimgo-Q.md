# 1. forge-std dependency should be removed from TimeswapV2Token

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L109

`console.log` should be removed before deploying.

# 2. misleading docs of ReentrancyGuard

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L21

@notice Reverts when balanceTarget is not zero.

but there's no balanceTarget, this comment should be removed.

# 3. misleading numberOfPairs function in TimeswapV2OptionFactory and TimeswapV2PoolFactory

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L36
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L52

`numberOfPairs` function return length of getByIndex, but the `getByIndex` list is not modified in create.

Remove this numberOfPairs function and getByIndex variable, or add a write `getByIndex.push(optionPair)` in create.

