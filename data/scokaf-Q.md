# 1: FLOATING PRAGMA

Vulnerability details

### Impact

Contracts should be deployed using the same compiler version/flags with which they have been tested. Locking the floating pragma, i.e. by not using ^ in pragma solidity ^0.8.8, ensures that contracts do not accidentally get deployed using an older compiler version with unfixed bugs.

For reference, see https://swcregistry.io/docs/SWC-103

## Proof of Concept


https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2 

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L2 

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L4 

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L4 

All contracts in the V2-Token src/structs folder

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs 

## Tools Used

Manual Analysis

### Recommended Mitigation Steps

Remove ^ in “pragma solidity ^0.8.8” and change it to “pragma solidity 0.8.8” to be consistent with the rest of the contracts.


# 2: USE A MORE RECENT VERSION OF SOLIDITY

Vulnerability details

### Context:

All contracts

## Description:

For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions; see https://github.com/ethereum/solidity/blob/develop/Changelog.md

## Tools Used

Manual Analysis

### Recommended Mitigation Steps

Old version of Solidity is used , newer version can be used (0.8.17)
