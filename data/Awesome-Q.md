# 1. Add whitespace between comment

To increase the readability of comment codes add at least 1 space at the beginning of single-line comments. If you are using multi-line comments add at least 1 space/newline at the beginning and end.

Here are a few examples of lousy comment spacing:

```solidity
//This is a comment with no whitespace at the beginning

/*This is a comment with no whitespace at the beginning */

/* This is a comment with whitespace at the beginning but not the end*/
```

Here are a few examples of good comment spacing:

```solidity
// This is a comment with a whitespace at the beginning

/* This is a comment with a whitespace at the beginning */

/*
 * This is a comment with a whitespace at the beginning
 */

/*
This comment has a newline
*/
```

Affected lines:

- [BytesLib.sol#L1](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L1)

- [BytesLib.sol#L55-L56](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L55-L56)

- [BytesLib.sol#L59](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L59)

- [BytesLib.sol#L62-L63](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L62-L63)
- [BytesLib.sol#L1](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L1)

# 2. Use mixedCase

It is recommended to use the mixedCase naming convention for improving the readability of source code.

This convention involves combining words with no spaces, with the first letter of each word capitalized except for the first word.

For more information, see the Solidity style guide at the following links:

- [Naming Convention For Function Names](https://docs.soliditylang.org/en/v0.5.3/style-guide.html#function-names)

- [Naming Convention For Local and State Variable Names](https://docs.soliditylang.org/en/v0.5.3/style-guide.html#local-and-state-variable-names.)

Affected lines of code:

- [BytesLib.sol#L33](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L33)

# 3. inadequate NatSpec

Contracts can use the Ethereum Natural Language Specification Format (NatSpec) to provide detailed documentation for functions, return variables, and other elements of the contract. This is done using a special type of comment within the contract code.

You can find more information about NatSpec and its usage in Solidity contracts at the following link [NatSpec Format](https://docs.soliditylang.org/en/v0.8.16/natspec-format.html)

Affected code:

- [FeesPosition.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol)

# 4. Use the `delete` operator to clear variables, rather than assigning a value of zero.

To clear variables, consider using the `delete` operator rather than assigning to zero, because this conveys the intention more clearly and is more idiomatic.

As an example on [line 685](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L685) you can refactor the code like so:

```solidity
Line 685:    delete pool.long1Balance;
```

Affected line of code:

- [Pool.sol#L685](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L685)
- [FullMath.sol#L166](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L166)

# 5. Unspecific Compiler Version Pragma

It is generally not recommended to use floating pragmas (i.e. pragmas that do not specify a specific compiler version) in contracts that are not intended to be used as libraries.

This is because using floating pragmas in application contracts can pose a security risk.

For example, a known vulnerable compiler version may be selected by mistake, or security tools might revert to an older compiler version that produces a different EVM compilation than the one intended to be deployed on the blockchain.

To avoid these potential issues, consider specifying a specific compiler version in your pragmas.

So instead of using a floating pragma like `pragma solidity ^0.8.0;`, it is better to use a concrete compiler version like `pragma solidity 0.8.4;`.

More information can be found in the following links:

- [Consensys Audit of 1inch](https://consensys.net/diligence/audits/2020/12/1inch-liquidity-protocol/#unspecific-compiler-version-pragma)
- [Solidity docs](https://docs.soliditylang.org/en/latest/layout-of-source-files.html#version-pragma)
- [Solidity Specific](https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/)

Affected line of code:

- [TimeswapV2LiquidityToken.sol#L2](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2)
- [TimeswapV2Token.sol#L2](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L2)

# 6. Unused returns

Remove unused returns to improve clarity

Affected line:

- [Math.sol#L88-L90](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L88-L90)

# 7. Code headers

Consider using this code header for your codebase layout for readability

- https://github.com/transmissions11/headers

```solidity
/*//////////////////////////////////////////////////////////////
                           EXAMPLE TEXT
//////////////////////////////////////////////////////////////*/
```

# 8. Fix typos to improve readability

```solidity
File: /packages/v2-library/src/FullMath.sol
Line  40:    /// @param addendA0 The least signficant part of addendA.
...
Line 250:    // correct result modulo 2**256. Since the precoditions guarantee
```

Consider making the following changes to [FullMath.sol](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol):

- `signficant` To `significant` ([Line 40](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L40))
- `precoditions` To `preconditions` ([Line 250](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L250))

```solidity
File: /packages/v2-pool/src/structs/Pool.sol
Line 155:    /// @param to The receipient of the liquidity positions.
...
Line 168:    // Update the fee growth and fees of receipient.
...
Line 178:    /// @param to The receipient of the transaction fees.
Line 179:    /// @param long0Fees The amount of long0 position fees transferrred.
Line 180:    /// @param long1Fees The amount of long1 position fees transferrred.
Line 181:    /// @param shortFees The amount of short position fees transferrred.
```

Consider making the following changes to [Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol):

- `receipient` To `recipient` ([Line 155](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L155), [Line 168](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L168), [Line 178](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L178))
- `transferrred` To `transferred` ([Line 179-181](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L179-L181))

```solidity
File: /packages/v2-pool/src/TimeswapV2Pool.sol
Line 222:    /// @dev Transfer long0 positions, long1 positions, and/or short positions to the receipients.
...
Line 225:    /// @param long0To The receipient of long0 positions.
Line 226:    /// @param long1To The receipient of long1 positions.
Line 227:    /// @param shortTo The receipient of short positions.
...
Line 332:    // Transfer the positions to the receipients.
...
Line 399:    // Transfer short positions to the receipient.
...
Line 446:    // Transfer the positions to the receipients.
...
Line 486:    // Transfer the positions to the receipients.
```

Consider making the following changes to [TimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol):

- `receipients` To `recipients` ([Line 222](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L222), [Line 332](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L332), [Line 446](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L446), [Line 486](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L486))
- `receipient` To `recipient` ([Line 225-227](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L225-L227), [Line 399](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L399))

```solidity
File: /packages/v2-pool/src/libraries/ConstantProduct.sol
Line 394:    /// @return sqrtDiscriminant The square root disriminant calculated.
```

Consider making the following change to [ConstantProduct.sol](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol):

- `disriminant` To `discriminant` ([Line 394](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L394))


