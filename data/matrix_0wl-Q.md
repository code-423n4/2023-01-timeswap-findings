# Summary

## Low Risk and Non-Critical Issues

## Non-Critical Issues

|      | Issue                                                                                            |
| ---- | :----------------------------------------------------------------------------------------------- |
| NC-1 | Separate tasks into different functions would improve the code's maintainability and readability |
| NC-2 | Natspec comments should be increased in contracts                                                |
| NC-3 | Omissions in events                                                                              |
| NC-4 | Use a more recent version of solidity                                                            |
| NC-5 | For functions, follow solidity standard naming conventions                                       |
| NC-6 | Lines are too long                                                                               |
| NC-7 | Functions not used internally could be marked external                                           |

### [NC-1] Separate tasks into different functions would improve the code's maintainability and readability

The function `getWithCheck` is doing two separate tasks: getting the option pair and checking if it's zero address. It would be better to separate these two tasks into different functions, this would improve the code's maintainability and readability.

#### **Proof Of Concept**

```solidity
File: packages/v2-option/src/libraries/OptionFactory.sol
37    function getWithCheck(address optionFactory, address token0, address token1) internal view returns (address optionPair) {
38        optionPair = get(optionFactory, token0, token1);
39        if (optionPair == address(0)) Error.zeroAddress();
```

[OptionFactory.sol#L37](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionFactory.sol#L37)

### [NC-2] Natspec comments should be increased in contracts

#### **Context:**

All Contracts

#### **Description:**

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.

In complex projects such as TimeSwap, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

#### **Recommendation**

NatSpec comments should be increased in contracts

### [NC-3] Omissions in events

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters.

The events should include the new value and old value where possible.

#### **Proof Of Concept**

Events with no old value:

```solidity
File: packages/v2-pool/src/base/OwnableTwoSteps.sol

31:         emit SetOwner(pendingOwner);

41:         emit AcceptOwner(msg.sender);

```

[OwnableTwoSteps.sol#L31](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L31)

[OwnableTwoSteps.sol#L41](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L41)

### [NC-4] Use a more recent version of solidity

#### **Context:**

All Contracts

#### **Description:**

For security, it is best practice to use the latest Solidity version.

For the security fix list in the versions.

https://github.com/ethereum/solidity/blob/develop/Changelog.md

#### **Recommendation**

Old version of Solidity is used , newer version can be used `(0.8.17)`

### [NC-5] For functions, follow solidity standard naming conventions

#### **Context:**

All Contracts

#### **Description:**

Solidity standard naming convention for internal and private functions: the mixedCase format starting with an underscore (\_mixedCase starting with an underscore)

#### **Recommendation**

Use solidity standard naming convention for internal and private functions

### [NC-6] Lines are too long

#### **Context:**

All Contracts

#### **Description:**

Usually lines in source code are limited to 80 characters.

[Reference](https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length)

#### **Recommendation**

The lines should be split when they reach that length.

### [NC-7] Functions not used internally could be marked external

#### **Proof Of Concept**

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

36:     function totalSupply() public view override returns (uint256) {

```

[ERC1155Enumerable.sol#L36](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L36)

## Low Issues

|     | Issue                                                                                                  |
| --- | :----------------------------------------------------------------------------------------------------- |
| L-1 | Critical changes should use two-step procedure                                                         |
| L-2 | No need to use the `internal` visibility on the function `getWithCheck` since it is a library function |

### [L-1] Critical changes should use two-step procedure

The critical procedures should be two step process.

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

#### **Proof Of Concept**

```solidity
File: packages/v2-pool/src/base/OwnableTwoSteps.sol

23:     function setPendingOwner(address chosenPendingOwner) external override {

```

[OwnableTwoSteps.sol#L23](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L23)

See similar findings in previous Code4rena contests for reference:

[code4arena example](https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure)

[code4arena example](https://code4rena.com/reports/2022-11-non-fungible#l-05-critical-address-changes-should-use-two-step-procedure)

### [L-2] No need to use the `internal` visibility on the function `getWithCheck` since it is a library function

There is no need to use the `internal` visibility on the function `getWithCheck` since it is a library function and it is not intended to be called from outside of the contract.

#### **Proof Of Concept**

```solidity
File: packages/v2-option/src/libraries/OptionFactory.sol
37    function getWithCheck(address optionFactory, address token0, address token1) internal view returns (address optionPair) {
```

[OptionFactory.sol#L37](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionFactory.sol#L37)
