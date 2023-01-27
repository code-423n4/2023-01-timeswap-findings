# 1. Regular addition/subtraction assignment saves gas

Using standard addition or subtraction assignment (`x = x + y` or `x = x - y`) rather than the shorthand versions (`x += y` or `x -= y`) saves gas. This can save approximately 22 gas per occurrence.

Affected line of code:

- [FullMath.sol#L163](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L163)
- [Option.sol#L62-L63](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L62-L63)
- [Option.sol#L65-L66](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L65-L66)
- [Option.sol#L68-L69](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L68-L69)
- [Option.sol#L103-L104](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L103-L104)
- [Option.sol#L108-L109](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L108-L109)
- [Option.sol#L112](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L112)
- [Option.sol#L141-L142](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L141-L142)
- [Option.sol#L173-L175](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L173-L175)
- [Option.sol#L177-L179](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L177-L179)
- [Option.sol#L208-L209](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L208-L209)

# 2. Uncheck arithmetics operations that can’t underflow/overflow

Solidity version 0.8+ has an implicit overflow and underflow check on unsigned integers.

When an overflow or an underflow isn’t possible, some gas can be saved using an unchecked block.

For example, the code at [FeesPosition.sol#L52-L56](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L52-L56)
can be refactored to as:

```solidity
File: /src/structs/FeesPosition.sol
    function mint(FeesPosition storage feesPosition, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) internal {
        unchecked {
            feesPosition.long0Fees += long0Fees;
            feesPosition.long1Fees += long1Fees;
            feesPosition.shortFees += shortFees;
        }
    }
```

This is affected in multiple files:

- [FeesPosition.sol#L52-L56](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L52-L56)

# 3. `>`/`<` Is Cheaper Than `>=`/`<=`

Using the `>`/`<` operator requires fewer opcodes and therefore costs less gas than using the `>=`/`<=` operator. This is demonstrated in the tweet linked below:

https://twitter.com/GalloDaSballo/status/1543729467465629696

This optimization can be applied to the following line of code:

- [BytesLib.sol#L13](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L13)
- [FullMath.sol#L189](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L189)
