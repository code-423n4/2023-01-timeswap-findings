# Summary
## Non critical
|ID     | Finding| Instances |
|:----: | :---           |   :----:         |
|1       | Checks for enums are unnecessary | 3 |
|2       | Remove libraries that are never used | 1 |
|3       | Functions should be grouped according to their visibility and ordered| - |
|4       | Unspecific compiler version pragma| 4 |
|5       | Miscellaneous| 1 |


# Details
## Non critical
## 1 Checks for enums are unnecessary
The checks in the enum libraries to check if the value of the enum exists are unnecessary. It's not possible to input an enum value that does not exist. EVM will revert if you try that
- [Position.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Position.sol)
- [v2-option/Transaction.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol)
- [v2-pool/Transaction.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol)

## 2 Remove libraries that are never used
[Duration.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/Duration.sol)
## 3 Functions should be grouped according to their visibility and ordered
According to the solidity style guide functions should be ordered like the following:
- constructor 
- receive function (if exists) 
- fallback function (if exists) 
- external 
- public 
- internal 
- private

For example in [TimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol), constructor is placed after a function.
## 4 Unspecific compiler version pragma
Avoid floating pragmas for non-library contracts. If the pragma version is specific, a know vulnerable compiler version may accidently be selected when deploying.
Used in:
- [TimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol)
- [TimeswapV2Token.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol)
- [IERC1155Enumerable.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol)
- [ERC1155Enumerable.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol)
## Miscellaneous
### Array is never updated but length is read
[TimeswapV2PoolFactory.sol#L33](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L33)