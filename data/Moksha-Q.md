### Inappropriate Revert Reasons

There’s a standard sanity check to maintain a unique `optionPair` in the pools. However, for every true case of sanity check the error reason would be `zeroAddress` which is misleading and should be changed to something like pair already exist.

Affected Line
[https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63)

### The override specifier can be omitted

The override keyword can be omitted since the project uses solidity version 0.8.8. See ref
[https://github.com/ethereum/solidity/blob/v0.8.8/Changelog.md](https://github.com/ethereum/solidity/blob/v0.8.8/Changelog.md)
Also, use the latest version of solidity

### Mismatch of Implementation with specification and typo

There is a named return variable `poolPair` in specification see line (https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L41) and while implementing TimeswapV2PoolFactory.sol (https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L59) has `pair` instead of `poolPair` . Furthermore, a similar kind of inconsistency is also with the get() function of the same file (https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L29). Similarly, there is a small typo `@return/@param` in the documentation see (https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L40)of ITimeswapV2PoolFactory.sol.

### Use curly braces for If/Else

It’s generally a good practice to use curly braces for if/else blocks. It significantly increases code readability and potentially lowers the possibility of bugs. Like popular goto fail bug(outside this project context)