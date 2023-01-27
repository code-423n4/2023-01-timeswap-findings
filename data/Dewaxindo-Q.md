# USE OF BLOCK.TIMESTAMP

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. 
Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

## Proof of Concept
Navigate to the following contract.

- [TimeswapV2Pool.sol#L83](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L83)
- [TimeswapV2Pool.sol#L212](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L212)
- [TimeswapV2Pool.sol#L261](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L261)
- [TimeswapV2Pool.sol#L387](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L387)

## Recommended Mitigation Steps
Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state(collect,leverage,delevrage).

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. 
It is sometimes recommended to use `block.number` and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. 
Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.


# USE A MORE RECENT VERSION OF SOLIDITY

## Context:

All contracts

## Description:

For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md

## Recommendation:

Old version of Solidity is used , newer version can be used (0.8.17)