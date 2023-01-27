## [YO NC-1] Constants should be defined rather than using magic numbers

### Handle
yosuke

## Vulnerability details
### Impact

### Proof of Concept
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L13
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Position.sol#L23
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol#L37
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol#L43
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol#L49



### Recommended Mitigation Steps

## [YO NC-2] Different pragma solids

### Handle
yosuke

## Vulnerability details
### Impact

### Proof of Concept
v2-token/src/interfaces/IERC1155Enumerable.sol
pragma solidity ^0.8.0;

All other files
pragma solidity =0.8.8;

### Recommended Mitigation Steps
Use the same solidity version.



## [YO LOW-1] **Unspecific compiler version pragma**

### Handle
yosuke

## Vulnerability details
### Impact

### Proof of Concept
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L4

### Recommended Mitigation Steps
ex)
before
```solidity=
pragma solidity ^0.8.0;
```
after
```solidity=
pragma solidity 0.8.0;
```