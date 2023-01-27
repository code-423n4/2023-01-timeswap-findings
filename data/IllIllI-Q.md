## Summary

### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | Vulnerable versions of packages are being used | 1 | 

Total: 1 instances over 1 issues


### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [N&#x2011;01] | Import declarations should import specific identifiers, rather than the whole file | 2 | 
| [N&#x2011;02] | Unused file | 3 | 
| [N&#x2011;03] | Remove `import` for forge-std | 1 | 
| [N&#x2011;04] | `public` functions not called by the contract should be declared `external` instead | 1 | 
| [N&#x2011;05] | `constant`s should be defined rather than using magic numbers | 51 | 
| [N&#x2011;06] | Events that mark critical parameter changes should contain both the old and the new value | 1 | 
| [N&#x2011;07] | Use a more recent version of solidity | 18 | 
| [N&#x2011;08] | Constant redefined elsewhere | 2 | 
| [N&#x2011;09] | Inconsistent spacing in comments | 5 | 
| [N&#x2011;10] | Lines are too long | 103 | 
| [N&#x2011;11] | Inconsistent method of specifying a floating pragma | 6 | 
| [N&#x2011;12] | Non-library/interface files should use fixed compiler versions, not floating ones | 2 | 
| [N&#x2011;13] | Typos | 77 | 
| [N&#x2011;14] | File is missing NatSpec | 3 | 
| [N&#x2011;15] | NatSpec is incomplete | 63 | 
| [N&#x2011;16] | Not using the named return variables anywhere in the function is confusing | 55 | 
| [N&#x2011;17] | Consider using `delete` rather than assigning zero to clear values | 10 | 
| [N&#x2011;18] | Contracts should have full test coverage | 1 | 
| [N&#x2011;19] | Large or complicated code bases should implement fuzzing tests | 1 | 
| [N&#x2011;20] | Function ordering does not follow the Solidity style guide | 18 | 
| [N&#x2011;21] | Contract does not follow the Solidity style guide's suggested layout ordering | 3 | 

Total: 426 instances over 21 issues





## Low Risk Issues

### [L&#x2011;01]  Vulnerable versions of packages are being used
This project's specific package versions are vulnerable to the specific CVEs listed below. Consider switching to more recent versions of these packages that don't have these vulnerabilities

*There is 1 instance of this issue:*

```solidity
File: Various Files

/// @audit Vulnerabilities:
///          
```



- [CVE-2022-35961](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-35961) - MEDIUM - OpenZeppelin Contracts is a library for secure smart contract development. The functions `ECDSA.recover` and `ECDSA.tryRecover` are vulnerable to a kind of signature malleability due to accepting EIP-2098 compact signatures in addition to the traditional 65 byte signature format. This is only an issue for the functions that take a single `bytes` argument, and not the functions that take `r, v, s` or `r, vs` as separate arguments. The potentially affected contracts are those that implement signature reuse or replay protection by marking the signature itself as used rather than the signed message or a nonce included in it. A user may take a signature that has already been submitted, submit it again in a different form, and bypass this protection. The issue has been patched in 4.7.3. (@openzeppelin/contracts >=4.1.0 <4.7.3)
```

```

## Non-critical Issues

### [N&#x2011;01]  Import declarations should import specific identifiers, rather than the whole file
Using import declarations of the form `import {<identifier_name>} from "some/file.sol"` avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation

*There are 2 instances of this issue:*

```solidity
File: packages/v2-token/src/interfaces/IERC1155Enumerable.sol

6:    import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L6

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

5:    import "forge-std/console.sol";

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L5

### [N&#x2011;02]  Unused file
The file is never imported by any other source file. If the file is needed for tests, it should be moved to a test directory

*There are 3 instances of this issue:*

```solidity
File: packages/v2-library/src/CatchError.sol

1:    //SPDX-License-Identifier: Unlicense

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/CatchError.sol#L1

```solidity
File: packages/v2-pool/src/libraries/Fee.sol

1:    //SPDX-License-Identifier: Unlicense

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/Fee.sol#L1

```solidity
File: packages/v2-pool/src/libraries/PoolPair.sol

1:    //SPDX-License-Identifier: Unlicense

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolPair.sol#L1

### [N&#x2011;03]  Remove `import` for forge-std
Test code should not be mixed in with production code. The production version should be extended and have its functions overridden for testing purposes

*There is 1 instance of this issue:*

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

5:    import "forge-std/console.sol";

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L5

### [N&#x2011;04]  `public` functions not called by the contract should be declared `external` instead
Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents' functions and change the visibility from `external` to `public`.

*There is 1 instance of this issue:*

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

66:       function positionOf(address owner, TimeswapV2TokenPosition calldata timeswapV2TokenPosition) public view returns (uint256 amount) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L66

### [N&#x2011;05]  `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*There are 51 instances of this issue:*

```solidity
File: packages/v2-library/src/BytesLib.sol

/// @audit 31
13:           require(_length + 31 >= _length, "slice_overflow");

/// @audit 0x40
23:                   tempBytes := mload(0x40)

/// @audit 31
33:                   let lengthmod := and(_length, 31)

/// @audit 0x20
39:                   let mc := add(add(tempBytes, lengthmod), mul(0x20, iszero(lengthmod)))

/// @audit 0x20
45:                       let cc := add(add(add(_bytes, lengthmod), mul(0x20, iszero(lengthmod))), _start)

/// @audit 0x20
47:                       mc := add(mc, 0x20)

/// @audit 0x20
48:                       cc := add(cc, 0x20)

/// @audit 0
20:               case 0 {

/// @audit 0x40
61:                   tempBytes := mload(0x40)

/// @audit 0x40
/// @audit 0x20
66:                   mstore(0x40, add(tempBytes, 0x20))

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L13

```solidity
File: packages/v2-library/src/CatchError.sol

/// @audit 4
/// @audit 32
/// @audit 4
/// @audit 4
15:           if ((length - 4) % 32 == 0 && bytes4(reason) == selector) return BytesLib.slice(reason, 4, length - 4);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/CatchError.sol#L15

```solidity
File: packages/v2-library/src/FullMath.sol

/// @audit 0
79:               let mm := mulmod(multiplicand, multiplier, not(0))

/// @audit 1
93:               quotient := add(div(sub(0, divisor), divisor), 1)

/// @audit 1
226:                  twos := add(div(sub(0, twos), twos), 1)

/// @audit 3
236:              inv = (3 * divisor) ^ 2;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L79

```solidity
File: packages/v2-library/src/StrikeConversion.sol

/// @audit 128
/// @audit 128
17:           return zeroToOne ? FullMath.mulDiv(amount, strike, uint256(1) << 128, roundUp) : FullMath.mulDiv(amount, uint256(1) << 128, strike, roundUp);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/StrikeConversion.sol#L17

```solidity
File: packages/v2-option/src/enums/Position.sol

/// @audit 3
23:           if (uint256(position) >= 3) revert InvalidPosition();

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/enums/Position.sol#L23

```solidity
File: packages/v2-option/src/enums/Transaction.sol

/// @audit 3
55:           if (uint256(transaction) >= 3) revert InvalidTransaction();

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/enums/Transaction.sol#L55

```solidity
File: packages/v2-pool/src/enums/Transaction.sol

/// @audit 4
53:           if (uint256(transaction) >= 4) revert InvalidTransaction();

/// @audit 4
58:           if (uint256(transaction) >= 4) revert InvalidTransaction();

/// @audit 4
63:           if (uint256(transaction) >= 4) revert InvalidTransaction();

/// @audit 4
68:           if (uint256(transaction) >= 4) revert InvalidTransaction();

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/enums/Transaction.sol#L53

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

/// @audit 96
30:           return (uint256(liquidity) << 96).div(rate, roundUp);

/// @audit 192
39:           return FullMath.mulDiv(uint256(liquidity).unsafeMul(duration), uint256(rate), uint256(1) << 192, roundUp);

/// @audit 96
260:          return FullMath.mulDiv(uint256(rate), longAmount, uint256(1) << 96, roundUp).toUint160();

/// @audit 192
269:          return FullMath.mulDiv(shortAmount, uint256(1) << 192, uint256(rate).unsafeMul(duration), roundUp).toUint160();

/// @audit 96
280:              numerator = uint256(liquidity) << 96;

/// @audit 192
316:          deltaRate = FullMath.mulDiv(shortAmount, uint256(1) << 192, denominator, !isAdd).toUint160();

/// @audit 96
332:              numerator = uint256(liquidity) << 96;

/// @audit 192
344:          return FullMath.mulDiv(uint256(numerator), uint256(deltaRate), uint256(1) << 192, roundUp);

/// @audit 16
374:          uint256 adjustment = (uint256(1) << 16).unsafeSub(transactionFee);

/// @audit 112
378:              ? FullMath.mulDiv(liquidity, uint256(1) << 112, uint256(rate).unsafeMul(adjustment), false)

/// @audit 176
379:              : FullMath.mulDiv(uint256(liquidity).unsafeMul(duration), rate, (uint256(1) << 176).unsafeMul(adjustment), false);

/// @audit 16
380:          uint256 negativeB2 = FullMath.mulDiv(sumAmount, uint256(1) << 16, adjustment, false);

/// @audit 174
/// @audit 16
/// @audit 16
404:          uint256 denominator = isShort ? (uint256(1) << 174).unsafeMul((uint256(1) << 16).unsafeSub(transactionFee)) : uint256(rate).unsafeMul((uint256(1) << 16).unsafeSub(transactionFee));

/// @audit 114
406:          (uint256 a0, uint256 a1) = isShort ? FullMath.mul512(uint256(liquidity).unsafeMul(duration), rate) : FullMath.mul512(liquidity, uint256(1) << 114);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L30

```solidity
File: packages/v2-pool/src/libraries/FeeCalculation.sol

/// @audit 128
41:           return globalFeeGrowth != lastFeeGrowth ? FullMath.mulDiv(liquidity, uint256(1) << 128, globalFeeGrowth.unsafeSub(lastFeeGrowth), false) : 0;

/// @audit 16
/// @audit 16
48:           return FullMath.mulDiv(amount, (uint256(1) << 16), (uint256(1) << 16).unsafeSub(fee), true);

/// @audit 16
/// @audit 16
55:           return FullMath.mulDiv(amount, (uint256(1) << 16).unsafeSub(fee), uint256(1) << 16, false);

/// @audit 16
62:           return FullMath.mulDiv(amount, fee, uint256(1) << 16, true);

/// @audit 16
69:           return FullMath.mulDiv(amount, fee, (uint256(1) << 16).unsafeSub(fee), true);

/// @audit 128
76:           return FullMath.mulDiv(feeAmount, uint256(1) << 128, liquidity, false);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/FeeCalculation.sol#L41

### [N&#x2011;06]  Events that mark critical parameter changes should contain both the old and the new value
This should especially be done if the new value is not required to be different from the old value

*There is 1 instance of this issue:*

```solidity
File: packages/v2-pool/src/base/OwnableTwoSteps.sol

/// @audit setPendingOwner()
31:           emit SetOwner(pendingOwner);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L31

### [N&#x2011;07]  Use a more recent version of solidity
Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions

*There are 18 instances of this issue:*

```solidity
File: packages/v2-library/src/FullMath.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L2

```solidity
File: packages/v2-option/src/libraries/OptionFactory.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/libraries/OptionFactory.sol#L2

```solidity
File: packages/v2-option/src/structs/Option.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L2

```solidity
File: packages/v2-option/src/TimeswapV2OptionFactory.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2OptionFactory.sol#L2

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L2

```solidity
File: packages/v2-pool/src/base/OwnableTwoSteps.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L2

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L2

```solidity
File: packages/v2-pool/src/libraries/ConstantSum.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantSum.sol#L2

```solidity
File: packages/v2-pool/src/libraries/DurationCalculation.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L2

```solidity
File: packages/v2-pool/src/libraries/DurationWeight.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationWeight.sol#L2

```solidity
File: packages/v2-pool/src/libraries/FeeCalculation.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/FeeCalculation.sol#L2

```solidity
File: packages/v2-pool/src/libraries/PoolFactory.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L2

```solidity
File: packages/v2-pool/src/structs/LiquidityPosition.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L2

```solidity
File: packages/v2-pool/src/structs/Pool.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L2

```solidity
File: packages/v2-pool/src/TimeswapV2PoolFactory.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L2

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

2:    pragma solidity =0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L2

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L2

### [N&#x2011;08]  Constant redefined elsewhere
Consider defining in only one contract so that values cannot become out of sync when only one location is updated. A [cheap way](https://medium.com/coinmonks/gas-cost-of-solidity-library-functions-dbe0cedd4678) to store constants in a single location is to create an `internal constant` in a `library`. If the variable is a local cache of another contract's value, consider making the cache variable internal or private, which will require external users to query the contract with the source of truth, so that callers don't get out of sync.

*There are 2 instances of this issue:*

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

/// @audit seen in packages/v2-pool/src/TimeswapV2PoolFactory.sol 
48:       uint256 public immutable override transactionFee;

/// @audit seen in packages/v2-pool/src/TimeswapV2PoolFactory.sol 
50:       uint256 public immutable override protocolFee;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L48

### [N&#x2011;09]  Inconsistent spacing in comments
Some lines use `// x` and some use `//x`. The instances below point out the usages that don't follow the majority, within each file

*There are 5 instances of this issue:*

```solidity
File: packages/v2-library/src/BytesLib.sol

55:                   //update free-memory pointer

56:                   //allocating the array padded to 32 bytes like the compiler does now

59:               //if we want a zero-length slice let's just return a zero-length array

62:                   //zero out the 32 bytes slice we are about to return

63:                   //we need to do it because Solidity does not garbage collect

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L55

### [N&#x2011;10]  Lines are too long
Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over [164](https://github.com/aizatto/character-length) characters, the lines below should be split when they reach that length

*There are 103 instances of this issue:*

```solidity
File: packages/v2-option/src/interfaces/ITimeswapV2Option.sol

159:      function mint(TimeswapV2OptionMintParam calldata param) external returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data);

169:      function burn(TimeswapV2OptionBurnParam calldata param) external returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data);

190:      function collect(TimeswapV2OptionCollectParam calldata param) external returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount, bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L159

```solidity
File: packages/v2-option/src/structs/Option.sol

95:           if (transaction == TimeswapV2OptionMint.GivenTokensAndLongs) shortAmount = StrikeConversion.combine(token0AndLong0Amount = amount0, token1AndLong1Amount = amount1, strike, false);

134:          if (transaction == TimeswapV2OptionBurn.GivenTokensAndLongs) shortAmount = StrikeConversion.combine(token0AndLong0Amount = amount0, token1AndLong1Amount = amount1, strike, true);

191:      function collect(Option storage option, uint256 strike, TimeswapV2OptionCollect transaction, uint256 amount) internal returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L95

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

27:   import {TimeswapV2OptionMintCallbackParam, TimeswapV2OptionBurnCallbackParam, TimeswapV2OptionSwapCallbackParam, TimeswapV2OptionCollectCallbackParam} from "./structs/CallbackParam.sol";

118:          (token0AndLong0Amount, token1AndLong1Amount, shortAmount) = option.mint(param.strike, param.long0To, param.long1To, param.shortTo, param.transaction, param.amount0, param.amount1);

198:      function swap(TimeswapV2OptionSwapParam calldata param) external override noDelegateCall returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, bytes memory data) {

235:          Error.checkEnough(IERC20(param.isLong0ToLong1 ? token1 : token0).balanceOf(address(this)), param.isLong0ToLong1 ? currentProcess.balance1Target : currentProcess.balance0Target);

246:      function collect(TimeswapV2OptionCollectParam calldata param) external override noDelegateCall returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount, bytes memory data) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L27

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol

13:       function timeswapV2PoolBurnChoiceCallback(TimeswapV2PoolBurnChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol#L13

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol

14:       function timeswapV2PoolDeleverageChoiceCallback(TimeswapV2PoolDeleverageChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L14

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol

14:       function timeswapV2PoolLeverageChoiceCallback(TimeswapV2PoolLeverageChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L14

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol

14:       function timeswapV2PoolMintChoiceCallback(TimeswapV2PoolMintChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L14

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol

6:    import {TimeswapV2PoolCollectParam, TimeswapV2PoolMintParam, TimeswapV2PoolBurnParam, TimeswapV2PoolDeleverageParam, TimeswapV2PoolLeverageParam, TimeswapV2PoolRebalanceParam} from "../structs/Param.sol";

81:       event Mint(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);

115:      event Deleverage(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);

126:      event Leverage(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address long0To, address long1To, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount);

138:      event Rebalance(uint256 indexed strike, uint256 indexed maturity, address indexed caller, address to, bool isLong0ToLong1, uint256 long0Amount, uint256 long1Amount);

204:      function protocolFeesEarned(uint256 strike, uint256 maturity) external view returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees);

280:      function mint(TimeswapV2PoolMintParam calldata param) external returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);

307:      function burn(TimeswapV2PoolBurnParam calldata param) external returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);

333:      function deleverage(TimeswapV2PoolDeleverageParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);

344:      function deleverage(TimeswapV2PoolDeleverageParam calldata param, uint96 durationForward) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);

353:      function leverage(TimeswapV2PoolLeverageParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);

364:      function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L6

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

51:       function calculateGivenLiquidityDelta(uint160 rate, uint160 deltaLiquidity, uint96 duration, bool isAdd) internal pure returns (uint256 longAmount, uint256 shortAmount) {

66:       function calculateGivenLiquidityLong(uint160 rate, uint256 longAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 shortAmount) {

81:       function calculateGivenLiquidityShort(uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 longAmount) {

313:      function getNewSqrtInterestRateGivenShort(uint160 liquidity, uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) private pure returns (uint160 newRate, uint160 deltaRate) {

334:          return roundUp ? FullMath.mulDiv(numerator, deltaRate, uint256(rate), true).div(newRate, true) : FullMath.mulDiv(numerator, deltaRate, uint256(newRate), false).div(rate, false);

356:      function getShortOrLongFromGivenSum(uint160 liquidity, uint160 rate, uint256 sumAmount, uint96 duration, uint256 transactionFee, bool isShort) private pure returns (uint256 amount) {

373:      function calculateNegativeB(uint160 liquidity, uint160 rate, uint256 sumAmount, uint96 duration, uint256 transactionFee, bool isShort) private pure returns (uint256 negativeB) {

404:          uint256 denominator = isShort ? (uint256(1) << 174).unsafeMul((uint256(1) << 16).unsafeSub(transactionFee)) : uint256(rate).unsafeMul((uint256(1) << 16).unsafeSub(transactionFee));

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L51

```solidity
File: packages/v2-pool/src/libraries/ConstantSum.sol

19:       function calculateGivenLongIn(uint256 strike, uint256 longAmountIn, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountOut, uint256 longFees) {

30:       function calculateGivenLongOut(uint256 strike, uint256 longAmountOut, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountIn, uint256 longFees) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantSum.sol#L19

```solidity
File: packages/v2-pool/src/libraries/FeeCalculation.sol

27:       function update(uint160 liquidity, uint256 feeGrowth, uint256 protocolFees, uint256 fees, uint256 protocolFee) internal pure returns (uint256 newFeeGrowth, uint256 newProtocolFees) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/FeeCalculation.sol#L27

```solidity
File: packages/v2-pool/src/structs/Param.sol

6:    import {TimeswapV2PoolMint, TimeswapV2PoolBurn, TimeswapV2PoolDeleverage, TimeswapV2PoolLeverage, TimeswapV2PoolRebalance, TransactionLibrary} from "../enums/Transaction.sol";

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Param.sol#L6

```solidity
File: packages/v2-pool/src/structs/Pool.sol

25:   import {TimeswapV2PoolMint, TimeswapV2PoolBurn, TimeswapV2PoolDeleverage, TimeswapV2PoolLeverage, TimeswapV2PoolRebalance, TransactionLibrary} from "../enums/Transaction.sol";

27:   import {TimeswapV2PoolCollectParam, TimeswapV2PoolMintParam, TimeswapV2PoolBurnParam, TimeswapV2PoolDeleverageParam, TimeswapV2PoolLeverageParam, TimeswapV2PoolRebalanceParam} from "./Param.sol";

28:   import {TimeswapV2PoolMintChoiceCallbackParam, TimeswapV2PoolBurnChoiceCallbackParam, TimeswapV2PoolDeleverageChoiceCallbackParam, TimeswapV2PoolLeverageChoiceCallbackParam} from "./CallbackParam.sol";

103:          shortAmount = ConstantProduct.getShort(pool.liquidity, pool.sqrtInterestRate, DurationCalculation.unsafeDurationFromNowToMaturity(maturity, blockTimestamp), false);

183:      function transferFees(Pool storage pool, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees, uint96 blockTimestamp) external {

533:              TimeswapV2PoolDeleverageChoiceCallbackParam({strike: param.strike, maturity: param.maturity, longAmount: longAmount, shortAmount: shortAmount, data: param.data})

639:              (pool.long0FeeGrowth, pool.long0ProtocolFees) = FeeCalculation.update(pool.liquidity, pool.long0FeeGrowth, pool.long0ProtocolFees, long0Fees, protocolFee);

653:              (pool.long1FeeGrowth, pool.long1ProtocolFees) = FeeCalculation.update(pool.liquidity, pool.long1FeeGrowth, pool.long1ProtocolFees, long1Fees, protocolFee);

665:      function rebalance(Pool storage pool, TimeswapV2PoolRebalanceParam memory param, uint256 transactionFee, uint256 protocolFee) external returns (uint256 long0Amount, uint256 long1Amount) {

697:              (pool.long1FeeGrowth, pool.long1ProtocolFees) = FeeCalculation.update(pool.liquidity, pool.long1FeeGrowth, pool.long1ProtocolFees, longFees, protocolFee);

724:              (pool.long0FeeGrowth, pool.long0ProtocolFees) = FeeCalculation.update(pool.liquidity, pool.long0FeeGrowth, pool.long0ProtocolFees, longFees, protocolFee);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L25

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

31:   import {TimeswapV2PoolCollectParam, TimeswapV2PoolMintParam, TimeswapV2PoolBurnParam, TimeswapV2PoolDeleverageParam, TimeswapV2PoolLeverageParam, TimeswapV2PoolRebalanceParam, ParamLibrary} from "./structs/Param.sol";

32:   import {TimeswapV2PoolMintChoiceCallbackParam, TimeswapV2PoolMintCallbackParam, TimeswapV2PoolBurnChoiceCallbackParam, TimeswapV2PoolBurnCallbackParam, TimeswapV2PoolDeleverageChoiceCallbackParam, TimeswapV2PoolDeleverageCallbackParam, TimeswapV2PoolLeverageCallbackParam, TimeswapV2PoolLeverageChoiceCallbackParam, TimeswapV2PoolRebalanceCallbackParam} from "./structs/CallbackParam.sol";

34:   import {TimeswapV2PoolMint, TimeswapV2PoolBurn, TimeswapV2PoolDeleverage, TimeswapV2PoolLeverage, TimeswapV2PoolRebalance, TransactionLibrary} from "./enums/Transaction.sol";

123:      function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view override returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

128:      function protocolFeesEarned(uint256 strike, uint256 maturity) external view override returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {

184:      function collectProtocolFees(TimeswapV2PoolCollectParam calldata param) external override noDelegateCall returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) {

192:          (long0Amount, long1Amount, shortAmount) = pools[param.strike][param.maturity].collectProtocolFees(param.long0Requested, param.long1Requested, param.shortRequested);

202:      function collectTransactionFees(TimeswapV2PoolCollectParam calldata param) external override noDelegateCall returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) {

231:      function collect(uint256 strike, uint256 maturity, address long0To, address long1To, address shortTo, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) private {

240:      function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

267:          if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;

271:          if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;

274:          uint256 shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;

293:          if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);

295:          if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);

297:          Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);

305:      function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

335:          if (long0Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long0To, TimeswapV2OptionPosition.Long0, long0Amount);

338:          if (long1Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long1To, TimeswapV2OptionPosition.Long1, long1Amount);

365:      function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

387:          (long0Amount, long1Amount, shortAmount, data) = pools[param.strike][param.maturity].deleverage(param, transactionFee, protocolFee, blockTimestamp(durationForward));

393:          if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;

397:          if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;

404:              TimeswapV2PoolDeleverageCallbackParam({strike: param.strike, maturity: param.maturity, long0Amount: long0Amount, long1Amount: long1Amount, shortAmount: shortAmount, data: data})

411:          if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);

413:          if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);

421:      function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

426:      function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

440:          (long0Amount, long1Amount, shortAmount, data) = pools[param.strike][param.maturity].leverage(param, transactionFee, protocolFee, blockTimestamp(durationForward));

448:          if (long0Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long0To, TimeswapV2OptionPosition.Long0, long0Amount);

450:          if (long1Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long1To, TimeswapV2OptionPosition.Long1, long1Amount);

454:              TimeswapV2PoolLeverageCallbackParam({strike: param.strike, maturity: param.maturity, long0Amount: long0Amount, long1Amount: long1Amount, shortAmount: shortAmount, data: data})

469:      function rebalance(TimeswapV2PoolRebalanceParam calldata param) external override noDelegateCall returns (uint256 long0Amount, uint256 long1Amount, bytes memory data) {

511:              ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), param.isLong0ToLong1 ? TimeswapV2OptionPosition.Long0 : TimeswapV2OptionPosition.Long1),

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L31

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol

42:       function transferFeesFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external;

59:       function collect(TimeswapV2LiquidityTokenCollectParam calldata param) external returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees, bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L42

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

22:   import {TimeswapV2LiquidityTokenMintCallbackParam, TimeswapV2LiquidityTokenBurnCallbackParam, TimeswapV2LiquidityTokenCollectCallbackParam} from "./structs/CallbackParam.sol";

70:       function transferTokenPositionFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata timeswapV2LiquidityTokenPosition, uint160 liquidityAmount) external {

75:       function transferFeesFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external override {

182:      function collect(TimeswapV2LiquidityTokenCollectParam calldata param) external returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees, bytes memory data) {

244:                  (, address poolPair) = PoolFactoryLibrary.getWithCheck(optionFactory, poolFactory, timeswapV2LiquidityTokenPosition.token0, timeswapV2LiquidityTokenPosition.token1);

246:                  (long0FeeGrowth, long1FeeGrowth, shortFeeGrowth) = ITimeswapV2Pool(poolPair).feeGrowth(timeswapV2LiquidityTokenPosition.strike, timeswapV2LiquidityTokenPosition.maturity);

270:              (, address poolPair) = PoolFactoryLibrary.getWithCheck(optionFactory, poolFactory, timeswapV2LiquidityTokenPosition.token0, timeswapV2LiquidityTokenPosition.token1);

272:              (long0FeeGrowth, long1FeeGrowth, shortFeeGrowth) = ITimeswapV2Pool(poolPair).feeGrowth(timeswapV2LiquidityTokenPosition.strike, timeswapV2LiquidityTokenPosition.maturity);

277:          (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) = feesPosition.feesEarnedOf(uint160(balanceOf(owner, id)), long0FeeGrowth, long1FeeGrowth, shortFeeGrowth);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L22

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

87:               long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + param.long0Amount;

117:              long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + param.long1Amount;

146:              shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + param.shortAmount;

186:          if (param.long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);

189:          if (param.long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);

192:          if (param.shortAmount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);

205:          if (param.long0Amount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.long0To, TimeswapV2OptionPosition.Long0, param.long0Amount);

213:          if (param.shortAmount != 0) ITimeswapV2Option(optionPair).transferPosition(param.strike, param.maturity, param.shortTo, TimeswapV2OptionPosition.Short, param.shortAmount);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L87

### [N&#x2011;11]  Inconsistent method of specifying a floating pragma
Some files use `>=`, some use `^`. The instances below are examples of the method that has the fewest instances for a specific version. Note that using `>=` without also specifying `<=` will lead to failures to compile, or external project incompatability, when the major version changes and there are breaking-changes, so `^` should be preferred regardless of the instance counts

*There are 6 instances of this issue:*

```solidity
File: packages/v2-token/src/structs/CallbackParam.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/CallbackParam.sol#L2

```solidity
File: packages/v2-token/src/structs/FeesPosition.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol#L2

```solidity
File: packages/v2-token/src/structs/Param.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Param.sol#L2

```solidity
File: packages/v2-token/src/structs/Position.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Position.sol#L2

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L2

### [N&#x2011;12]  Non-library/interface files should use fixed compiler versions, not floating ones

*There are 2 instances of this issue:*

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

2:    pragma solidity ^0.8.8;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L2

### [N&#x2011;13]  Typos

*There are 77 instances of this issue:*

```solidity
File: packages/v2-library/src/FullMath.sol

/// @audit signficant
40:       /// @param addendA0 The least signficant part of addendA.

/// @audit precoditions
250:              // correct result modulo 2**256. Since the precoditions guarantee

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L40

```solidity
File: packages/v2-library/src/StrikeConversion.sol

/// @audit oneToZero
22:       /// @param amount The amount ot be converted. Token0 amount when zeroToOne. Token1 amount when oneToZero.

/// @audit toekn1
40:       /// @dev When oneToZero, given a larger base amount, and toekn1 amount, get the difference token0 amount.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/StrikeConversion.sol#L22

```solidity
File: packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol

/// @audit receipients
12:       /// @dev The token0 and token1 will already transferred to the receipients.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol#L12

```solidity
File: packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol

/// @audit receipients
12:       /// @dev The token0 and token1 will already transferred to the receipients.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol#L12

```solidity
File: packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol

/// @audit receipients
12:       /// @dev The long0 positions, long1 positions, and/or short positions will already minted to the receipients.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol#L12

```solidity
File: packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol

/// @audit receipients
12:       /// @dev The long0 positions or long1 positions will already minted to the receipients.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol#L12

```solidity
File: packages/v2-option/src/interfaces/ITimeswapV2Option.sol

/// @audit receipient
18:       /// @param to The address of the receipient of the position.

/// @audit receipient
27:       /// @param long0To The address of the receipient of long token0 position.

/// @audit receipient
28:       /// @param long1To The address of the receipient of long token1 position.

/// @audit receipient
29:       /// @param shortTo The address of the receipient of short position.

/// @audit receipient
49:       /// @param token0To The address of the receipient of token0.

/// @audit receipient
50:       /// @param token1To The address of the receipient of token1.

/// @audit receipient
69:       /// @param tokenTo The address of the receipient of token0 or token1.

/// @audit receipient
70:       /// @param longTo The address of the receipient of long token0 or long token1.

/// @audit receipient
91:       /// @param token0To The address of the receipient of token0.

/// @audit receipient
92:       /// @param token1To The address of the receipient of token1.

/// @audit receipient
145:      /// @param to The address of the receipient of the position.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L18

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

/// @audit maturies
47:       /// @dev mapping of all option state for all strikes and maturies.

/// @audit overidden
69:       // Can be overidden for testing purposes.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L47

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol

/// @audit receipient
10:       /// @dev The short positions will already be minted to the receipient.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L10

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol

/// @audit receipients
10:       /// @dev The long0 positions and long1 positions will already be minted to the receipients.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L10

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol

/// @audit receipient
10:       /// @dev The liquidity positionss will already be minted to the receipient.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L10

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol

/// @audit receipient
10:       /// @dev The long0 positions or long1 positions will already be minted to the receipient.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol#L10

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol

/// @audit receipeint
14:       /// @param to The receipeint of liquidity position.

/// @audit receipeint
22:       /// @param to The receipeint of fees position.

/// @audit receipient
32:       /// @param long0To The receipient of long0 position fees.

/// @audit receipient
33:       /// @param long1To The receipient of long1 position fees.

/// @audit receipient
34:       /// @param shortTo The receipient of short position fees.

/// @audit receipient
54:       /// @param long0To The receipient of long0 position fees.

/// @audit receipient
55:       /// @param long1To The receipient of long1 position fees.

/// @audit receipient
56:       /// @param shortTo The receipient of short position fees.

/// @audit receipient
76:       /// @param to The receipient of liquidity positions.

/// @audit receipient
87:       /// @param long0To The receipient of long0 positions.

/// @audit receipient
88:       /// @param long1To The receipient of long1 positions.

/// @audit receipient
89:       /// @param shortTo The receipient of short positions.

/// @audit receipient
111:      /// @param to The receipient of short positions.

/// @audit receipient
121:      /// @param long0To The receipient of long0 positions.

/// @audit receipient
122:      /// @param long1To The receipient of long1 positions.

/// @audit receipient
/// @audit ekse
132:      /// @param to If isLong0ToLong1 then receipient of long0 positions, ekse recipient of long1 positions.

/// @audit receipient
234:      /// @param to The receipient of the liquidity positions.

/// @audit receipient
242:      /// @param to The receipient of the transaction fees.

/// @audit transferrred
243:      /// @param long0Fees The amount of long0 position fees transferrred.

/// @audit transferrred
244:      /// @param long1Fees The amount of long1 position fees transferrred.

/// @audit transferrred
245:      /// @param shortFees The amount of short position fees transferrred.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L14

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

/// @audit liqudity
19:       /// @dev Reverts when there is not enough time value liqudity to receive when lending.

/// @audit disriminant
394:      /// @return sqrtDiscriminant The square root disriminant calculated.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L19

```solidity
File: packages/v2-pool/src/libraries/PoolFactory.sol

/// @audit retreived
27:       /// @return optionPair The retreived option pair address. Zero address if not deployed.

/// @audit retreived
28:       /// @return poolPair The retreived pool pair address. Zero address if not deployed.

/// @audit retreived
41:       /// @return optionPair The retreived option pair address.

/// @audit retreived
42:       /// @return poolPair The retreived pool pair address.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L27

```solidity
File: packages/v2-pool/src/libraries/PoolPair.sol

/// @audit doesn
19:       /// @dev Checks if the pool doesn not exist.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolPair.sol#L19

```solidity
File: packages/v2-pool/src/structs/Pool.sol

/// @audit receipient
155:      /// @param to The receipient of the liquidity positions.

/// @audit receipient
168:          // Update the fee growth and fees of receipient.

/// @audit receipient
178:      /// @param to The receipient of the transaction fees.

/// @audit transferrred
179:      /// @param long0Fees The amount of long0 position fees transferrred.

/// @audit transferrred
180:      /// @param long1Fees The amount of long1 position fees transferrred.

/// @audit transferrred
181:      /// @param shortFees The amount of short position fees transferrred.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L155

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

/// @audit overidden
81:       // Can be overidden for testing purposes.

/// @audit receipients
222:      /// @dev Transfer long0 positions, long1 positions, and/or short positions to the receipients.

/// @audit receipient
225:      /// @param long0To The receipient of long0 positions.

/// @audit receipient
226:      /// @param long1To The receipient of long1 positions.

/// @audit receipient
227:      /// @param shortTo The receipient of short positions.

/// @audit receipients
332:          // Transfer the positions to the receipients.

/// @audit receipient
399:          // Transfer short positions to the receipient.

/// @audit receipients
446:          // Transfer the positions to the receipients.

/// @audit receipients
486:          // Transfer the positions to the receipients.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L81

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

/// @audit overidden
69:       /// @dev Any additional condition to add token enumeration when overidden.

/// @audit overidden
74:       /// @dev Any additional condition to add token enumeration when overidden.

/// @audit overidden
103:      /// @dev Any additional condition to remove token enumeration when overidden.

/// @audit overidden
108:      /// @dev Any additional condition to remove token enumeration when overidden.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L69

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2Token.sol

/// @audit postion
27:       /// @dev mints TimeswapV2Token as per postion and amount

/// @audit postion
32:       /// @dev burns TimeswapV2Token as per postion and amount

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L27

```solidity
File: packages/v2-token/src/structs/Position.sol

/// @audit keccak
33:       /// @dev return keccak for key management for Token.

/// @audit keccak
38:       /// @dev return keccak for key management for Liquidity Token.

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Position.sol#L33

### [N&#x2011;14]  File is missing NatSpec

*There are 3 instances of this issue:*

```solidity
File: packages/v2-option/src/TimeswapV2OptionFactory.sol


```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2OptionFactory.sol

```solidity
File: packages/v2-pool/src/TimeswapV2PoolDeployer.sol


```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolDeployer.sol

```solidity
File: packages/v2-token/src/structs/FeesPosition.sol


```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/FeesPosition.sol

### [N&#x2011;15]  NatSpec is incomplete

*There are 63 instances of this issue:*

```solidity
File: packages/v2-library/src/CatchError.sol

/// @audit Missing: '@return'
10        /// @param reason The data being inquired upon.
11        /// @param selector The given conditional selector.
12:       function catchError(bytes memory reason, bytes4 selector) internal pure returns (bytes memory) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/CatchError.sol#L10-L12

```solidity
File: packages/v2-library/src/Error.sol

/// @audit Missing: '@param maturity'
123       /// @dev Reverts when an option of given strike and maturity is still inactive.
124       /// @param strike The chosen strike.
125:      function inactiveOptionChoice(uint256 strike, uint256 maturity) internal pure {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Error.sol#L123-L125

```solidity
File: packages/v2-library/src/FullMath.sol

/// @audit Missing: '@return'
113       /// @param quotient0 The least significant part of quotient.
114       /// @param quotient1 The most significant part of quotient.
115:      function div512(uint256 dividend0, uint256 dividend1, uint256 divisor) private pure returns (uint256 quotient0, uint256 quotient1) {

/// @audit Missing: '@return'
136       /// @param roundUp Round up the result when true. Round down if false.
137       /// @param quotient The quotient.
138:      function div512To256(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {

/// @audit Missing: '@return'
156       /// @param quotient0 The least significant part of quotient.
157       /// @param quotient1 The most significant part of quotient.
158:      function div512(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient0, uint256 quotient1) {

/// @audit Missing: '@return'
285       /// @param currentEstimate The current estimate of the iteration.
286       /// @param estimate The new estimate of the iteration.
287:      function sqrt512Estimate(uint256 value0, uint256 value1, uint256 currentEstimate) private pure returns (uint256 estimate) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L113-L115

```solidity
File: packages/v2-library/src/SafeCast.sol

/// @audit Missing: '@return'
16        /// @param value The uint256 number to be safecasted.
17        /// @param result The uint16 result.
18:       function toUint16(uint256 value) internal pure returns (uint16 result) {

/// @audit Missing: '@return'
25        /// @param value The uint256 number to be safecasted.
26        /// @param result The uint96 result.
27:       function toUint96(uint256 value) internal pure returns (uint96 result) {

/// @audit Missing: '@return'
34        /// @param value The uint256 number to be safecasted.
35        /// @param result The uint160 result.
36:       function toUint160(uint256 value) internal pure returns (uint160 result) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/SafeCast.sol#L16-L18

```solidity
File: packages/v2-library/src/StrikeConversion.sol

/// @audit Missing: '@return'
14        /// @param zeroToOne ZeroToOne if it is true. OneToZero if it is false.
15        /// @param roundUp Round up the result when true. Round down if false.
16:       function convert(uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) internal pure returns (uint256) {

/// @audit Missing: '@return'
24        /// @param toOne ToOne if it is true, ToZero if it is false.
25        /// @param roundUp Round up the result when true. Round down if false.
26:       function turn(uint256 amount, uint256 strike, bool toOne, bool roundUp) internal pure returns (uint256) {

/// @audit Missing: '@return'
33        /// @param strike The strike multiple conversion.
34        /// @param roundUp Round up the result when true. Round down if false.
35:       function combine(uint256 amount0, uint256 amount1, uint256 strike, bool roundUp) internal pure returns (uint256) {

/// @audit Missing: '@return'
44        /// @param zeroToOne ZeroToOne if it is true. OneToZero if it is false.
45        /// @param roundUp Round up the result when true. Round down if false.
46:       function dif(uint256 base, uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) internal pure returns (uint256) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/StrikeConversion.sol#L14-L16

```solidity
File: packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol

/// @audit Missing: '@return'
26        /// @dev Get the address of the option pair in the option pair enumeration list.
27        /// @param id The chosen index.
28:       function getByIndex(uint256 id) external view returns (address optionPair);

/// @audit Missing: '@return'
39        /// @param token1 The second ERC20 token address of the pair.
40        /// @param optionPair The address of the Timeswap V2 Option contract created.
41:       function create(address token0, address token1) external returns (address optionPair);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L26-L28

```solidity
File: packages/v2-option/src/interfaces/ITimeswapV2Option.sol

/// @audit Missing: '@return'
118       /// @dev Get the strike and maturity of the option in the option enumeration list.
119       /// @param id The chosen index.
120:      function getByIndex(uint256 id) external view returns (StrikeAndMaturity memory);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L118-L120

```solidity
File: packages/v2-option/src/libraries/Proportion.sol

/// @audit Missing: '@param roundUp'
/// @audit Missing: '@return'
7         /// @dev Get the balance proportion calculation.
8         /// @notice Round down the result.
9         /// @param multiplicand The multiplicand balance.
10        /// @param multiplier The multiplier balance.
11        /// @param divisor The divisor balance.
12:       function proportion(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) internal pure returns (uint256) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/libraries/Proportion.sol#L7-L12

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol

/// @audit Missing: '@param param'
8         /// @dev Returns the amount of long0 position and long1 positions chosen to be withdrawn.
9         /// @notice The StrikeConversion.combine of long0 position and long1 position must be less than or equal to long amount.
10        /// @return long0Amount Amount of long0 position to be withdrawn.
11        /// @return long1Amount Amount of long1 position to be withdrawn.
12        /// @return data The bytes of data to be sent to msg.sender.
13:       function timeswapV2PoolBurnChoiceCallback(TimeswapV2PoolBurnChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);

/// @audit Missing: '@param param'
18        /// @dev Require enough liquidity position by the msg.sender.
19        /// @return data The bytes of data to be sent to msg.sender.
20:       function timeswapV2PoolBurnCallback(TimeswapV2PoolBurnCallbackParam calldata param) external returns (bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol#L8-L13

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol

/// @audit Missing: '@param param'
8         /// @dev Returns the amount of long0 position and long1 positions chosen to be deposited to the pool.
9         /// @notice The StrikeConversion.combine of long0 position and long1 position must be greater than or equal to long amount.
10        /// @dev The short positions will already be minted to the receipient.
11        /// @return long0Amount Amount of long0 position to be deposited.
12        /// @return long1Amount Amount of long1 position to be deposited.
13        /// @param data The bytes of data to be sent to msg.sender.
14:       function timeswapV2PoolDeleverageChoiceCallback(TimeswapV2PoolDeleverageChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);

/// @audit Missing: '@param param'
/// @audit Missing: '@return'
16        /// @dev Require the transfer of long0 position and long1 position into the pool.
17        /// @param data The bytes of data to be sent to msg.sender.
18:       function timeswapV2PoolDeleverageCallback(TimeswapV2PoolDeleverageCallbackParam calldata param) external returns (bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L8-L14

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol

/// @audit Missing: '@param param'
8         /// @dev Returns the amount of long0 position and long1 positions chosen to be withdrawn.
9         /// @notice The StrikeConversion.combine of long0 position and long1 position must be less than or equal to long amount.
10        /// @dev The long0 positions and long1 positions will already be minted to the receipients.
11        /// @return long0Amount Amount of long0 position to be withdrawn.
12        /// @return long1Amount Amount of long1 position to be withdrawn.
13        /// @param data The bytes of data to be sent to msg.sender.
14:       function timeswapV2PoolLeverageChoiceCallback(TimeswapV2PoolLeverageChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);

/// @audit Missing: '@param param'
/// @audit Missing: '@return'
16        /// @dev Require the transfer of short position into the pool.
17        /// @param data The bytes of data to be sent to msg.sender.
18:       function timeswapV2PoolLeverageCallback(TimeswapV2PoolLeverageCallbackParam calldata param) external returns (bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L8-L14

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol

/// @audit Missing: '@param param'
8         /// @dev Returns the amount of long0 position and long1 positions chosen to be deposited to the pool.
9         /// @notice The StrikeConversion.combine of long0 position and long1 position must be greater than or equal to long amount.
10        /// @dev The liquidity positionss will already be minted to the receipient.
11        /// @return long0Amount Amount of long0 position to be deposited.
12        /// @return long1Amount Amount of long1 position to be deposited.
13        /// @param data The bytes of data to be sent to msg.sender.
14:       function timeswapV2PoolMintChoiceCallback(TimeswapV2PoolMintChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);

/// @audit Missing: '@param param'
/// @audit Missing: '@return'
16        /// @dev Require the transfer of long0 position, long1 position, and short position into the pool.
17        /// @param data The bytes of data to be sent to msg.sender.
18:       function timeswapV2PoolMintCallback(TimeswapV2PoolMintCallbackParam calldata param) external returns (bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L8-L14

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol

/// @audit Missing: '@param param'
/// @audit Missing: '@return'
8         /// @dev When Long0ToLong1, require the transfer of long0 position into the pool.
9         /// @dev When Long1ToLong0, require the transfer of long1 position into the pool.
10        /// @dev The long0 positions or long1 positions will already be minted to the receipient.
11        /// @param data The bytes of data to be sent to msg.sender.
12:       function timeswapV2PoolRebalanceCallback(TimeswapV2PoolRebalanceCallbackParam calldata param) external returns (bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol#L8-L12

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol

/// @audit Missing: '@return'
39        /// @param option The address of the option contract used by the pool.
40        /// @param poolPair The address of the Timeswap V2 Pool contract created.
41:       function create(address option) external returns (address poolPair);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L39-L41

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol

/// @audit Missing: '@return'
156       /// @dev Get the strike and maturity of the pool in the pool enumeration list.
157       /// @param id The chosen index.
158:      function getByIndex(uint256 id) external view returns (StrikeAndMaturity memory);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L156-L158

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

/// @audit Missing: '@return'
27        /// @param rate The pool's squared root Interest Rate.
28        /// @param roundUp Rounds up the result when true. Rounds down the result when false.
29:       function getLong(uint160 liquidity, uint160 rate, bool roundUp) internal pure returns (uint256) {

/// @audit Missing: '@return'
36        /// @param duration The time duration in seconds.
37        /// @param roundUp Rounds up the result when true. Rounds down the result when false.
38:       function getShort(uint160 liquidity, uint160 rate, uint96 duration, bool roundUp) internal pure returns (uint256) {

/// @audit Missing: '@param isAdd'
153       /// @dev Update the new square root interest rate given change in long positions in base denomination.
154       /// @param liquidity The amount of liquidity of the pool.
155       /// @param rate The pool's squared root Interest Rate.
156       /// @param longAmount The amount of long positions.
157       /// @param duration The time duration in seconds.
158       /// @param transactionFee The fee that will be adjusted in the transaction.
159       /// @return newRate The new squared root Interest Rate.
160       /// @return shortAmount The amount of short positions to withdraw when depositing long positions in base denomination.
161       /// The amount of short positions to deposit when withdrawing long positions in base denomination.
162       /// @return fees The amount of short positions fee when depositing long positions in base denomination.
163       /// The amount of long positions fee in base denominations fee when withdrawing long positions in base denomination.
164       function updateGivenLong(
165           uint160 liquidity,
166           uint160 rate,
167           uint256 longAmount,
168           uint96 duration,
169           uint256 transactionFee,
170           bool isAdd
171:      ) internal pure returns (uint160 newRate, uint256 shortAmount, uint256 fees) {

/// @audit Missing: '@param isAdd'
184       /// @dev Update the new square root interest rate given change in short positions.
185       /// @param liquidity The amount of liquidity of the pool.
186       /// @param rate The pool's squared root Interest Rate.
187       /// @param shortAmount The amount of short positions.
188       /// @param duration The time duration in seconds.
189       /// @param transactionFee The fee that will be adjusted in the transaction.
190       /// @return newRate The new squared root Interest Rate.
191       /// @return longAmount The amount of long positions in base denomination to withdraw when depositing short positions.
192       /// The amount of long positions in base denomination to deposit when withdrawing short positions.
193       /// @return fees The amount of long positions fee in base denominations when depositing short positions.
194       /// The amount of short positions fee when withdrawing short positions.
195       function updateGivenShort(
196           uint160 liquidity,
197           uint160 rate,
198           uint256 shortAmount,
199           uint96 duration,
200           uint256 transactionFee,
201           bool isAdd
202:      ) internal pure returns (uint160 newRate, uint256 longAmount, uint256 fees) {

/// @audit Missing: '@return'
257       /// @param longAmount The amount of long in base denomination change..
258       /// @param roundUp Round up the result when true. Round down the result when false.
259:      function getLiquidityGivenLong(uint160 rate, uint256 longAmount, bool roundUp) private pure returns (uint160) {

/// @audit Missing: '@return'
266       /// @param duration The time duration in seconds.
267       /// @param roundUp Round up the result when true. Round down the result when false.
268:      function getLiquidityGivenShort(uint160 rate, uint256 shortAmount, uint96 duration, bool roundUp) private pure returns (uint160) {

/// @audit Missing: '@return'
275       /// @param longAmount The amount long positions in base denomination change.
276       /// @param isAdd Long positions increase when true. Long positions decrease when false.
277:      function getNewSqrtInterestRateGivenLong(uint160 liquidity, uint160 rate, uint256 longAmount, bool isAdd) private pure returns (uint160) {

/// @audit Missing: '@return'
327       /// @param newRate The new interest rate of the pool
328       /// @param roundUp Increases in square root interest rate when true. Decrease in square root interest rate when false.
329:      function getLongFromSqrtInterestRate(uint160 liquidity, uint160 rate, uint160 deltaRate, uint160 newRate, bool roundUp) private pure returns (uint256) {

/// @audit Missing: '@return'
340       /// @param duration The time duration in seconds.
341       /// @param roundUp Increases in square root interest rate when true. Decrease in square root interest rate when false.
342:      function getShortFromSqrtInterestRate(uint160 liquidity, uint160 deltaRate, uint96 duration, bool roundUp) private pure returns (uint256) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L27-L29

```solidity
File: packages/v2-pool/src/libraries/ConstantSum.sol

/// @audit Missing: '@return'
17        /// @param transactionFee The fee that will be adjusted in the transaction.
18        /// @param isLong0ToLong1 Deposit long0 positions when true. Deposit long1 positions when false.
19:       function calculateGivenLongIn(uint256 strike, uint256 longAmountIn, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountOut, uint256 longFees) {

/// @audit Missing: '@return'
28        /// @param transactionFee The fee that will be adjusted in the transaction.
29        /// @param isLong0ToLong1 Deposit long0 positions when true. Deposit long1 positions when false.
30:       function calculateGivenLongOut(uint256 strike, uint256 longAmountOut, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountIn, uint256 longFees) {

/// @audit Missing: '@return'
37        /// @param longAmountOut The long amount to be withdrawn.
38        /// @param isLong0ToLong1 Deposit long0 positions when true. Deposit long1 positions when false.
39:       function calculateGivenLongOutAlreadyAdjustFees(uint256 strike, uint256 longAmountOut, bool isLong0ToLong1) internal pure returns (uint256 longAmountIn) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantSum.sol#L17-L19

```solidity
File: packages/v2-pool/src/libraries/Duration.sol

/// @audit Missing: '@return'
9         /// @dev Reverts when the duration is too large.
10        /// @param duration The duration in seconds which is needed to be converted to the Duration type.
11:       function init(uint256 duration) internal pure returns (uint96) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/Duration.sol#L9-L11

```solidity
File: packages/v2-pool/src/libraries/DurationWeight.sol

/// @audit Missing: '@return'
13        /// @param shortAmount The amount of short withdrawn.
14        /// @param newShortFeeGrowth The newly updated short fee growth.
15:       function update(uint160 liquidity, uint256 shortFeeGrowth, uint256 shortAmount) internal pure returns (uint256 newShortFeeGrowth) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationWeight.sol#L13-L15

```solidity
File: packages/v2-pool/src/libraries/FeeCalculation.sol

/// @audit Missing: '@return'
38        /// @param lastFeeGrowth The previous global fee growth when owner enters.
39        /// @param globalFeeGrowth The current global fee growth.
40:       function getFees(uint160 liquidity, uint256 lastFeeGrowth, uint256 globalFeeGrowth) internal pure returns (uint256) {

/// @audit Missing: '@return'
45        /// @param amount The original amount.
46        /// @param fee The transaction fee rate.
47:       function addFees(uint256 amount, uint256 fee) internal pure returns (uint256) {

/// @audit Missing: '@return'
52        /// @param amount The original amount.
53        /// @param fee The transaction fee rate.
54:       function removeFees(uint256 amount, uint256 fee) internal pure returns (uint256) {

/// @audit Missing: '@return'
59        /// @param amount The amount with fees.
60        /// @param fee The transaction fee rate.
61:       function getFeesRemoval(uint256 amount, uint256 fee) internal pure returns (uint256) {

/// @audit Missing: '@return'
66        /// @param amount The amount with fees.
67        /// @param fee The transaction fee rate.
68:       function getFeesAdditional(uint256 amount, uint256 fee) internal pure returns (uint256) {

/// @audit Missing: '@return'
73        /// @param feeAmount The fee amount.
74        /// @param liquidity The current liquidity in the pool.
75:       function getFeeGrowth(uint256 feeAmount, uint160 liquidity) internal pure returns (uint256) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/FeeCalculation.sol#L38-L40

```solidity
File: packages/v2-pool/src/structs/LiquidityPosition.sol

/// @audit Missing: '@return'
31        /// @param long1FeeGrowth The current global long1 position fee growth to be compared.
32        /// @param shortFeeGrowth The current global short position fee growth to be compared.
33        function feesEarnedOf(
34            LiquidityPosition memory liquidityPosition,
35            uint256 long0FeeGrowth,
36            uint256 long1FeeGrowth,
37            uint256 shortFeeGrowth
38:       ) internal pure returns (uint256 long0Fee, uint256 long1Fee, uint256 shortFee) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L31-L38

```solidity
File: packages/v2-pool/src/structs/Pool.sol

/// @audit Missing: '@param transactionFee'
87        /// @dev Returns the amount of long0 and long1 adjusted for the protocol and transaction fee.
88        /// @param pool The state of the pool.
89        /// @return long0Amount The amount of long0 in the pool, adjusted for the protocol and transaction fee.
90        /// @return long1Amount The amount of long1 in the pool, adjusted for the protocol and transaction fee.
91:       function totalLongBalanceAdjustFees(Pool storage pool, uint256 transactionFee) external view returns (uint256 long0Amount, uint256 long1Amount) {

/// @audit Missing: '@param maturity'
/// @audit Missing: '@param blockTimestamp'
96        /// @dev Returns the amount of sum of long0 and long1 converted to base denomination in the pool.
97        /// @dev Returns the amount of short positions in the pool.
98        /// @param pool The state of the pool.
99        /// @return longAmount The amount of sum of long0 and long1 converted to base denomination in the pool.
100       /// @return shortAmount The amount of short in the pool.
101:      function totalPositions(Pool storage pool, uint256 maturity, uint96 blockTimestamp) external view returns (uint256 longAmount, uint256 shortAmount) {

/// @audit Missing: '@param maturity'
139       /// @dev Move short positions to short fee growth due to duration of the pool decreasing as time moves forward when pool is after maturity.
140       /// @param pool The state of the pool.
141       /// @param blockTimestamp The block timestamp.
142:      function updateDurationWeightAfterMaturity(Pool storage pool, uint256 maturity, uint96 blockTimestamp) private {

/// @audit Missing: '@param maturity'
175       /// @dev Transfer fees earned of the sender to another address.
176       /// @notice Does not transfer the liquidity positions of the sender.
177       /// @param pool The state of the pool.
178       /// @param to The receipient of the transaction fees.
179       /// @param long0Fees The amount of long0 position fees transferrred.
180       /// @param long1Fees The amount of long1 position fees transferrred.
181       /// @param shortFees The amount of short position fees transferrred.
182       /// @param blockTimestamp The current block timestamp.
183:      function transferFees(Pool storage pool, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees, uint96 blockTimestamp) external {

/// @audit Missing: '@param maturity'
251       /// @dev Collects the transaction fees of the pool.
252       /// @dev only liquidity provider can call this function.
253       /// @dev if the owner enters an amount which is greater than the fee amount they have earned, withdraw only the amount they have.
254       /// @param pool The state of the pool.
255       /// @param blockTimestamp The current block timestamp.
256       /// @param long0Requested The maximum amount of long0 positions wanted.
257       /// @param long1Requested The maximum amount of long1 positions wanted.
258       /// @param shortRequested The maximum amount of short positions wanted.
259       /// @return long0Amount The amount of long0 collected.
260       /// @return long1Amount The amount of long1 collected.
261       /// @return shortAmount The amount of short collected.
262       function collectTransactionFees(
263           Pool storage pool,
264           uint256 maturity,
265           uint256 long0Requested,
266           uint256 long1Requested,
267           uint256 shortRequested,
268           uint96 blockTimestamp
269:      ) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L87-L91

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol

/// @audit Missing: '@return'
24        /// @param owner The owner of the token
25        /// @param position The liquidity position
26:       function positionOf(address owner, TimeswapV2LiquidityTokenPosition calldata position) external view returns (uint256 amount);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L24-L26

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2Token.sol

/// @audit Missing: '@return'
16        /// @param owner The owner of the token
17        /// @param position type of option position (long0, long1, short)
18:       function positionOf(address owner, TimeswapV2TokenPosition calldata position) external view returns (uint256 amount);

/// @audit Missing: '@return'
32        /// @dev burns TimeswapV2Token as per postion and amount
33        /// @param param The TimeswapV2TokenBurnParam
34:       function burn(TimeswapV2TokenBurnParam calldata param) external returns (bytes memory data);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L16-L18

### [N&#x2011;16]  Not using the named return variables anywhere in the function is confusing
Consider changing the variable to be an unnamed one

*There are 55 instances of this issue:*

```solidity
File: packages/v2-library/src/Math.sol

/// @audit result
88:       function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/Math.sol#L88

```solidity
File: packages/v2-pool/src/structs/Pool.sol

/// @audit long0FeeGrowth
/// @audit long1FeeGrowth
/// @audit shortFeeGrowth
66:       function feeGrowth(Pool storage pool) external view returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {

/// @audit long0Fees
/// @audit long1Fees
/// @audit shortFees
75:       function feesEarnedOf(Pool storage pool, address owner) external view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

/// @audit long0ProtocolFees
/// @audit long1ProtocolFees
/// @audit shortProtocolFees
83:       function protocolFeesEarned(Pool storage pool) external view returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L66

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

/// @audit long0FeeGrowth
/// @audit long1FeeGrowth
/// @audit shortFeeGrowth
118:      function feeGrowth(uint256 strike, uint256 maturity) external view override returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {

/// @audit long0Fees
/// @audit long1Fees
/// @audit shortFees
123:      function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view override returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

/// @audit long0ProtocolFees
/// @audit long1ProtocolFees
/// @audit shortProtocolFees
128:      function protocolFeesEarned(uint256 strike, uint256 maturity) external view override returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {

/// @audit liquidityAmount
/// @audit long0Amount
/// @audit long1Amount
/// @audit shortAmount
/// @audit data
240:      function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit liquidityAmount
/// @audit long0Amount
/// @audit long1Amount
/// @audit shortAmount
/// @audit data
245       function mint(
246           TimeswapV2PoolMintParam calldata param,
247           uint96 durationForward
248:      ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit liquidityAmount
/// @audit long0Amount
/// @audit long1Amount
/// @audit shortAmount
/// @audit data
305:      function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit liquidityAmount
/// @audit long0Amount
/// @audit long1Amount
/// @audit shortAmount
/// @audit data
310       function burn(
311           TimeswapV2PoolBurnParam calldata param,
312           uint96 durationForward
313:      ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit long0Amount
/// @audit long1Amount
/// @audit shortAmount
/// @audit data
365:      function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit long0Amount
/// @audit long1Amount
/// @audit shortAmount
/// @audit data
370       function deleverage(
371           TimeswapV2PoolDeleverageParam calldata param,
372           uint96 durationForward
373:      ) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit long0Amount
/// @audit long1Amount
/// @audit shortAmount
/// @audit data
421:      function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit long0Amount
/// @audit long1Amount
/// @audit shortAmount
/// @audit data
426:      function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L118

### [N&#x2011;17]  Consider using `delete` rather than assigning zero to clear values
The `delete` keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic

*There are 10 instances of this issue:*

```solidity
File: packages/v2-pool/src/structs/LiquidityPosition.sol

93:               liquidityPosition.long0Fees = 0;

101:              liquidityPosition.long1Fees = 0;

109:              liquidityPosition.shortFees = 0;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L93

```solidity
File: packages/v2-pool/src/structs/Pool.sol

228:              pool.long0ProtocolFees = 0;

236:              pool.long1ProtocolFees = 0;

244:              pool.shortProtocolFees = 0;

633:                  pool.long0Balance = 0;

646:                  pool.long1Balance = 0;

685:                      pool.long1Balance = 0;

706:                      pool.long0Balance = 0;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L228

### [N&#x2011;18]  Contracts should have full test coverage
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.

*There is 1 instance of this issue:*

```solidity
File: Various Files


```

### [N&#x2011;19]  Large or complicated code bases should implement fuzzing tests
Large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts, should implement [fuzzing tests](https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05). Fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold. Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and fuzzers, with properly and extensively-written invariants, can close this testing gap significantly.

*There is 1 instance of this issue:*

```solidity
File: Various Files


```

### [N&#x2011;20]  Function ordering does not follow the Solidity style guide
According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :`constructor()`, `receive()`, `fallback()`, `external`, `public`, `internal`, `private`, but the cases below do not follow this pattern

*There are 18 instances of this issue:*

```solidity
File: packages/v2-library/src/FullMath.sol

/// @audit div512() came earlier
138:      function div512To256(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L138

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

/// @audit addOptionEnumerationIfNecessary() came earlier
65:       constructor() NoDelegateCall() {

/// @audit blockTimestamp() came earlier
76:       function getByIndex(uint256 id) external view override returns (StrikeAndMaturity memory) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L65

```solidity
File: packages/v2-pool/src/libraries/FeeCalculation.sol

/// @audit feeOverflow() came earlier
27:       function update(uint160 liquidity, uint256 feeGrowth, uint256 protocolFees, uint256 fees, uint256 protocolFee) internal pure returns (uint256 newFeeGrowth, uint256 newProtocolFees) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/FeeCalculation.sol#L27

```solidity
File: packages/v2-pool/src/structs/Pool.sol

/// @audit updateDurationWeightAfterMaturity() came earlier
158:      function transferLiquidity(Pool storage pool, address to, uint160 liquidityAmount, uint96 blockTimestamp) external {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L158

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

/// @audit lowerGuard() came earlier
77:       constructor() NoDelegateCall() {

/// @audit blockTimestamp() came earlier
89:       function getByIndex(uint256 id) external view override returns (StrikeAndMaturity memory) {

/// @audit hasLiquidity() came earlier
103:      function totalLiquidity(uint256 strike, uint256 maturity) external view override returns (uint160) {

/// @audit collect() came earlier
240:      function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit mint() came earlier
305:      function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit burn() came earlier
365:      function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit deleverage() came earlier
421:      function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit leverage() came earlier
469:      function rebalance(TimeswapV2PoolRebalanceParam calldata param) external override noDelegateCall returns (uint256 long0Amount, uint256 long1Amount, bytes memory data) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L77

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

/// @audit totalSupply() came earlier
41:       function tokenByIndex(uint256 index) external view override returns (uint256) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L41

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

/// @audit lowerGuard() came earlier
65:       function positionOf(address owner, TimeswapV2LiquidityTokenPosition calldata timeswapV2LiquidityTokenPosition) external view returns (uint256 amount) {

/// @audit _updateFeesPositions() came earlier
255:      function _additionalConditionAddTokenToOwnerEnumeration(address to, uint256 id) internal view override returns (bool) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L65

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

/// @audit lowerGuard() came earlier
66:       function positionOf(address owner, TimeswapV2TokenPosition calldata timeswapV2TokenPosition) public view returns (uint256 amount) {

/// @audit positionOf() came earlier
71:       function transferTokenPositionFrom(address from, address to, TimeswapV2TokenPosition calldata timeswapV2TokenPosition, uint256 amount) external override {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L66

### [N&#x2011;21]  Contract does not follow the Solidity style guide's suggested layout ordering
The [style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout) says that, within a contract, the ordering should be 1) Type declarations, 2) State variables, 3) Events, 4) Modifiers, and 5) Functions, but the contract(s) below do not follow this ordering

*There are 3 instances of this issue:*

```solidity
File: packages/v2-option/src/NoDelegateCall.sol

/// @audit function checkNotDelegateCall came earlier
34        modifier noDelegateCall() {
35            checkNotDelegateCall();
36            _;
37:       }

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/NoDelegateCall.sol#L34-L37

```solidity
File: packages/v2-pool/src/NoDelegateCall.sol

/// @audit function checkNotDelegateCall came earlier
34        modifier noDelegateCall() {
35            checkNotDelegateCall();
36            _;
37:       }

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/NoDelegateCall.sol#L34-L37

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

/// @audit function constructor came earlier
41:       mapping(bytes32 => uint96) private reentrancyGuards;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L41


___

## Excluded findings
These findings are excluded from awards calculations because there are publicly-available automated tools that find them. The valid ones appear here for completeness

## Summary

### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | Missing checks for `address(0x0)` when assigning values to `address` state variables | 4 | 

Total: 4 instances over 1 issues


### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [N&#x2011;01] | `constant`s should be defined rather than using magic numbers | 3 | 
| [N&#x2011;02] | Event is missing `indexed` fields | 3 | 

Total: 6 instances over 2 issues





## Low Risk Issues

### [L&#x2011;01]  Missing checks for `address(0x0)` when assigning values to `address` state variables

*There are 4 instances of this issue:*

```solidity
File: packages/v2-pool/src/base/OwnableTwoSteps.sol

/// @audit (valid but excluded finding)
19:           owner = chosenOwner;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L19

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

/// @audit (valid but excluded finding)
37:           optionFactory = chosenOptionFactory;

/// @audit (valid but excluded finding)
38:           poolFactory = chosenPoolFactory;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L37

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

/// @audit (valid but excluded finding)
42:           optionFactory = chosenOptionFactory;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L42

## Non-critical Issues

### [N&#x2011;01]  `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*There are 3 instances of this issue:*

```solidity
File: packages/v2-library/src/BytesLib.sol

/// @audit 0x40 - (valid but excluded finding)
/// @audit 31 - (valid but excluded finding)
/// @audit 31 - (valid but excluded finding)
57:                   mstore(0x40, and(add(mc, 31), not(31)))

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L57

### [N&#x2011;02]  Event is missing `indexed` fields
Index event fields make the field more quickly accessible [to off-chain tools](https://ethereum.stackexchange.com/questions/40396/can-somebody-please-explain-the-concept-of-event-indexing) that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*There are 3 instances of this issue:*

```solidity
File: packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol

/// @audit (valid but excluded finding)
7:        event SetOwner(address pendingOwner);

/// @audit (valid but excluded finding)
11:       event AcceptOwner(address owner);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol#L7

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol

/// @audit (valid but excluded finding)
13:       event TransferFees(address from, address to, TimeswapV2LiquidityTokenPosition position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L13
