QA1. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L47-L55
We need to make sure the two arrays, ``ids`` and ``amounts``, have the same length.
```
if(ids.length != amounts.length) revert ArrayLengthsNotEqual();

```

QA2. https://github.com/code-423n4/2023-01-timeswap/blame/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L79
We should check if ``from == msg.sender`` as well here
```
if (from != msg.sender && !isApprovedForAll(from, msg.sender)) revert NotApprovedToTransferFees();
```
