#### Zero address error thrown if _not_ zero address

##### Severity: Low

##### Context: 

- https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63

##### Description:

A check is made to revert if `pair != address(0)`, but the error being used in the case of revert is `Error.zeroAddress`, which is used throughout the codebase to indicate that the error is that a value is `address(0)`.