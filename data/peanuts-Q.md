### [N-01] The function collect() is not in the ITimeswapV2Pool.sol interface

The function collect() in TimeswapV2Pool.sol is not in the ITimeswapV2Pool.sol interface

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L231

### [N-02] Use _ for private functions

Use the underscore (_) for private functions to remove confusion. Right now, functions are written with the same name, which is hard to read.

mint(),burn(),leverage(),deleverage(), in TimeswapV2Pool.sol

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L245-L253

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L369-L381

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L309-L323

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L425-L435

### [N-03] Use a more recent version of solidity

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value. Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)

All contracts, for example: 

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L2

### [N-04] Function writing does not follow solidity style guide

Context:

All Contracts

Description:

Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private

within a grouping, place the view and pure functions last

### [N-05] Increase test coverage 
```
  - What is the overall line coverage percentage provided by your tests?: ~50 (`forge coverage` currently throws a "stack too deep" error on large codebases)
```
Insufficient test coverage of protocol. Due to its capacity, test coverage is expected to be 100%.

### [N-06] Don't use unsafe functions

The parameters passed into the unsafe functions have already been checked. However, there could still be an edge case which results in under/overflow.

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L95-L113

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L41-L43

Use OpenZeppelin's SafeMath library instead, or make sure that the Math library that stores the unsafe functions revert if there is an overflow.

### [N-07] Make sure the SPDX license is licensable 

Context: 

All contracts

```
//SPDX-License-Identifier: Unlicense
```