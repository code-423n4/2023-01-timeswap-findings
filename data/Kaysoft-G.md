## [GAS-01] REMOVE `console.log` statement

The `console.log` statement will cost extra gas. consider removing the console.log statement.

File: [packages/v2-token/src/TimeswapV2Token.sol#L109](https://github.com/code-423n4/2023-01-timeswap/blob/7cb8c18ba6d0871033ddf518dd79c4444e2f89cc/packages/v2-token/src/TimeswapV2Token.sol#L109)

```
console.log("reaches right before mint in timeswapv2Tokne::mint");
```

## [GAS-02] x = x + y cost less gas than x +=y for state variables. Same for x = x -y.

Files: 

- [packages/v2-token/src/base/ERC1155Enumerable.sol#L60](https://github.com/code-423n4/2023-01-timeswap/blob/7cb8c18ba6d0871033ddf518dd79c4444e2f89cc/packages/v2-token/src/base/ERC1155Enumerable.sol#L60)
- [packages/v2-token/src/base/ERC1155Enumerable.sol#L95](https://github.com/code-423n4/2023-01-timeswap/blob/7cb8c18ba6d0871033ddf518dd79c4444e2f89cc/packages/v2-token/src/base/ERC1155Enumerable.sol#L95)