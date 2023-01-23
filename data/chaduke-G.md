G1. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2OptionFactory.sol#L49
Replacing it by a zero address check for ``optionPair`` will do it. That is what the function did anyway.
```
if (optionPair != address(0)) revert OptionPairAlreadyExisted(token0, token1, optionPair);
```