- [Low](#low)
    - [**1. Mixing and Outdated compiler**](#1-mixing-and-outdated-compiler)
    - [**2. Use abstract for base contracts**](#2-use-abstract-for-base-contracts)
    - [**3. Avoid hardcoded values**](#3-avoid-hardcoded-values)
    - [**4. Integer overflow by unsafe casting**](#4-integer-overflow-by-unsafe-casting)

# Low

## **1. Mixing and Outdated compiler**

The pragma version used are:


```
pragma solidity =0.8.8;
pragma solidity ^0.8.0;
```

*Note that mixing pragma is not recommended. Because different compiler versions have different meanings and behaviors, it also significantly raises maintenance costs. As a result, depending on the compiler version selected for any given file, deployed contracts may have security issues.*

The minimum required version must be [0.8.17](https://github.com/ethereum/solidity/releases/tag/v0.8.17); otherwise, contracts will be affected by the following **important bug fixes**:

[0.8.9](https://blog.soliditylang.org/2021/09/29/solidity-0.8.9-release-announcement/):

- Immutables: Properly perform sign extension on signed immutables.
- User Defined Value Type: Fix storage layout of user defined value types for underlying types shorter than 32 bytes.

[0.8.13](https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/):
- Code Generator: Correctly encode literals used in `abi.encodeCall` in place of fixed bytes arguments.

[0.8.14](https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/):

- ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against `calldatasize()` in all cases.
- Override Checker: Allow changing data location for parameters only when overriding external functions.

[0.8.15](https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/)

- Code Generation: Avoid writing dirty bytes to storage when copying `bytes` arrays.
- Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

[0.8.16](https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/)

- Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

[0.8.17](https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/)

- Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

Apart from these, there are several minor bug fixes and improvements.

## **2. Use `abstract` for base contracts**

`abstract` contracts are contracts that have at least one function without its implementation. **An instance of an abstract cannot be created.**

**Reference:**

- https://docs.soliditylang.org/en/v0.6.2/contracts.html#abstract-contracts

**Affected source code:**

- [OwnableTwoSteps.sol:10](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L10)
- [NoDelegateCall.sol:6](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/NoDelegateCall.sol#L6)

## **3. Avoid hardcoded values**

It is not good practice to hardcode values, but if you are dealing with addresses much less, these can change between implementations, networks or projects, so it is convenient to remove these values from the source code.

**Affected source code:**

- [Base.sol:11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/lib/forge-std/src/Base.sol#L11)
- [Base.sol:15](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/lib/forge-std/src/Base.sol#L15)
- [Base.sol:27](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/lib/forge-std/src/Base.sol#L27)
- [StdUtils.sol:9](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/lib/forge-std/src/StdUtils.sol#L9)
- [Base.sol:11](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/lib/forge-std/src/Base.sol#L11)
- [Base.sol:15](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/lib/forge-std/src/Base.sol#L15)
- [Base.sol:17](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/lib/forge-std/src/Base.sol#L17)
- [Base.sol:29](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/lib/forge-std/src/Base.sol#L29)
- [StdUtils.sol:17](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/lib/forge-std/src/StdUtils.sol#L17)

## **4. Integer overflow by unsafe casting**

Keep in mind that the version of solidity used, despite being greater than `0.8`, does not prevent integer overflows during casting, it only does so in mathematical operations.

It is necessary to safely convert between the different numeric types.

**Recommendation:**

Use a [safeCast](https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast) from Open Zeppelin.

```javascript
    function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {
        return uint96(block.timestamp + durationForward); // truncation is desired
    }
```

**Affected source code:**

- [TimeswapV2Pool.sol:83](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L83)
