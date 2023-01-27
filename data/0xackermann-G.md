### Gas Optimizations Report
| No. | Issue |
| --- | --- |
| 1 | [Using `uint256` instead of `bool` in mappings is more gas efficient [146 gas per instance]](#G-01-using-uint256-instead-of-bool-in-mappings-is-more-gas-efficient-146-gas-per-instance)|
| 2 | [Use Custom Errors rather than revert() / require() strings to save deployment gas [68 gas per instance]](#G-02-use-custom-errors-rather-than-revert--require-strings-to-save-deployment-gas-68-gas-per-instance)|
| 3 | [Usage of uint/int smaller than 32 bytes (256 bits) incurs overhead](#G-03-usage-of-uintint-smaller-than-32-bytes-256-bits-incurs-overhead)|
| 4 | [Structs can be packed into fewer storage slots (20k gas)](#G-04-structs-can-be-packed-into-fewer-storage-slots-20k-gas)|
---
### [G-01] Using `uint256` instead of `bool` in mappings is more gas efficient [146 gas per instance]

---
**Description:**

OpenZeppelin uint256 and bool comparison: 

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled. The values being non-zero value makes deployment a bit more expensive.

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past.

**Recommendation:**

Use uint256(1) and uint256(2) instead of bool.

**Lines of Code:** 

- [TimeswapV2Option.sol#L53](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L53)

---
### [G-02] Use Custom Errors rather than revert() / require() strings to save deployment gas [68 gas per instance]

---
**Description:**

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they’re hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas.

<https://blog.soliditylang.org/2021/04/21/custom-errors/>

**Recommendation:**

Use the Custom Errors feature.

**Lines of Code:** 

- [BytesLib.sol#L13](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/BytesLib.sol#L13)
- [BytesLib.sol#L14](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/BytesLib.sol#L14)

---
### [G-03] Usage of uint/int smaller than 32 bytes (256 bits) incurs overhead

---
**Description:**

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so.

**Recommendation:**

Use a larger size then downcast where needed

**Lines of Code:** 

- [Math.sol#L59](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Math.sol#L59)

---
### [G-04] Structs can be packed into fewer storage slots (20k gas)

---
**Description:**

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.			

**Recommendation:**

Reorder the variables in the struct

**Lines of Code:** 

- [Position.sol#L12-L18](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L12-L18)
