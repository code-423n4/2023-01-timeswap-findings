### Total Low issues

| Number | Issues Details                                                                     | Context       |
|--------|------------------------------------------------------------------------------------|---------------|
| [L-01] | Low level calls with solidity version 0.8.14 and lower can result in optimiser bug | 12            |
| [L-02] | Integer overflow by unsafe casting                                                 | 1             |
| [L-03] | Need Fuzzing test                                                                  | All Contracts |
| [L-04] | Array lengths not checked                                                          | 2             |

### Total Non-Critical issues

| Number  | Issues Details                                                                            | Context       |
|---------|-------------------------------------------------------------------------------------------|---------------|
| [NC-01] | Lock pragmas to specific compiler version                                                 | 8             |
| [NC-02] | Constants in comparisons should appear on the left side                                   | All Contracts |
| [NC-03] | Use a more recent version of solidity                                                     | All Contracts |
| [NC-04] | Include `@return` parameters in NatSpec comments                                          | All Contracts |
| [NC-05] | Contracts should have full test coverage                                                  | All Contracts |
| [NC-06] | Function writing does not comply with the `Solidity Style Guide`                          | All Contracts |
| [NC-07] | Solidity compiler optimizations can be problematic                                        | 3             |
| [NC-08] | Add a timelock to critical functions                                                      | 1             |
| [NC-09] | Lines are too long                                                                        | 30            |
| [NC-10] | NatSpec comments should be increased in contracts                                         | All Contracts |
| [NC-11] | Generate perfect code headers every time                                                  | All Contracts |
| [NC-12] | For functions, follow Solidity standard naming conventions                                | All Contracts |
| [NC-13] | `Address(0)` checks                                                                       | 2             |
| [NC-14] | Use a more recent version of solidity                                                     | All Contracts |
| [NC-15] | Assembly Codes Specific – Should Have Comments                                            | 5             |
| [NC-16] | Events that mark critical parameter changes should contain both the old and the new value | 1             |
| [NC-17] | Consider using `delete` rather than assigning zero to clear values                        | 10            |
| [NC-18] | Add NatSpec Mapping comment                                                               | 10            |
| [NC-19] | Using vulnerable dependency of OpenZeppelin                                               | 2             |
| [NC-20] | Constants should be defined rather than using magic numbers                               | 18            |

## [L-01] Low level calls with solidity version 0.8.14 and lower can result in optimiser bug

The protocol is using low level calls with solidity version less then 0.8.14 which can result in optimizer bug. The Solidity developers have confirmed this [bug](https://github.com/ethereum/solidity-blog/blob/499ab8abc19391be7b7b34f88953a067029a5b45/_posts/2022-06-15-inline-assembly-memory-side-effects-bug.md) (which was introduced in Solidity 0.8.13) and fixed it in Solidity version 0.8.15. 

> Ref: https://medium.com/certora/overly-optimistic-optimizer-certora-bug-disclosure-2101e3f7994d

### Lines of code

- [BytesLib.sol:18](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L18)
- [CatchError.sol:17](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol#L17)
- [FullMath.sol:47](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L47)
- [FullMath.sol:63](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L63)
- [FullMath.sol:78](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L78)
- [FullMath.sol:92](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L92)
- [FullMath.sol:103](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L103)
- [FullMath.sol:199](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L199)
- [FullMath.sol:203](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L203)
- [FullMath.sol:214](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L214)
- [FullMath.sol:220](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L220)
- [FullMath.sol:225](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L225)

### Recommended Mitigation Steps

Use solidity version 0.8.15.

## [L-02] Integer overflow by unsafe casting

Keep in mind that the version of solidity used, despite being greater than 0.8, does not prevent integer overflows during casting, it only does so in mathematical operations.

It is necessary to safely convert between the different numeric types.

```solidity
    function blockTimestamp() internal view virtual returns (uint96) {
        return uint96(block.timestamp); // truncation is desired
    }
```

### Lines of code

- [TimeswapV2Option.sol:71](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L71)

### Recommended Mitigation Steps

Use [`SafeCast.toUint96()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/SafeCast.sol#L27) function to prevent unexpected overflows when casting from uint256.

## [L-03] Need Fuzzing test 

In total 6 contracts, 11 unchecked are used, the functions used are critical. For this reason, there must be fuzzing tests in the tests of the project. Not seen in tests.

### Lines of code

- [FullMath.sol:191](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L191)
- [Math.sol:15](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L15)
- [Math.sol:26](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L26)
- [Math.sol:37](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L37)
- [Math.sol:72](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L72)
- [ConstantProduct.sol:279](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L279)
- [ConstantProduct.sol:331](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L331)

### Recommended Mitigation Steps

Use fuzzing test like Echidna. As Alberto Cuesta Canada said: Fuzzing is not easy, the tools are rough, and the math is hard, but it is worth it. Fuzzing gives me a level of confidence in my smart contracts that I didn’t have before. Relying just on unit testing anymore and poking around in a testnet seems reckless now.

> Ref: https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05

## [L-04] Array lengths not checked 

If the length of the arrays are not required to be of the same length, user operations may not be fully executed.

```solidity
    function _beforeTokenTransfer(address, address from, address to, uint256[] memory ids, uint256[] memory amounts, bytes memory) internal virtual override {
        for (uint256 i; i < ids.length; ) {
            if (amounts[i] != 0) _addTokenEnumeration(from, to, ids[i], amounts[i]);

            unchecked {
                ++i;
            }
        }
    }
```

### Lines of code

- [ERC1155Enumerable.sol:47](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L47)
- [TimeswapV2LiquidityToken.sol:224](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L224)

### Recommended Mitigation Steps

```solidity
if (ids.length != amounts.length) revert();
```

## [NC-01] Lock pragmas to specific compiler version

Pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or EthPM package. Otherwise, the developer would need to manually update the pragma in order to compile locally. 

> Ref: https://swcregistry.io/docs/SWC-103

```solidity
    pragma solidity ^0.8.8;
```

### Lines of code 

- [TimeswapV2LiquidityToken.sol:2](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2)
- [TimeswapV2Token.sol:2](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L2)
- [ERC1155Enumerable.sol:4](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L4)
- [IERC1155Enumerable.sol:4](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L4)
- [CallbackParam.sol:2](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L2)
- [FeesPosition.sol:2](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L2)
- [Param.sol:2](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L2)
- [Position.sol:2](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol#L2)

### Recommended Mitigation Steps

Ethereum Smart Contract Best Practices - Lock pragmas to specific compiler version. 

> https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/

## [NC-02] Constants in comparisons should appear on the left side

Constants in comparisons should appear on the left side, doing so will prevent [typo bug](https://www.moserware.com/2008/01/constants-on-left-are-better-but-this.html).

```solidity
    if (value == 0) revert ModuloByZero();
```

### Lines of code 

- **All Contracts**

### Recommended Mitigation Steps

```solidity
    if (0 == value) revert ModuloByZero();
```

## [NC-03] Use a more recent version of solidity

Use a solidity version of at least 0.8.12 to get `string.concat()` instead of `abi.encodePacked(<string>,<string>)`.

> https://blog.soliditylang.org/2022/02/16/solidity-0.8.12-release-announcement/

```solidity
    pragma solidity =0.8.8;
```

```solidity
    pragma solidity ^0.8.8;
```

### Lines of code 

- **All Contract**

### Recommended Mitigation Steps

Consider upgrading to 0.8.12.

## [NC-04] Include `@return` parameters in NatSpec comments

If Return parameters are declared, you must prefix them with `/// @return`. Some code analysis programs do analysis by reading [NatSpec](https://docs.soliditylang.org/en/v0.8.15/natspec-format.html) details, if they can't see the `@return` tag, they do incomplete analysis.

### Lines of code

- **All Contracts**

### Recommended Mitigation Steps

Include the `@return` argument in the NatSpec comments.

## [NC-05] Contracts should have full test coverage

While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.

```solidity
  - What is the overall line coverage percentage provided by your tests?: ~50 (`forge coverage` currently throws a "stack too deep" error on large codebases)

```

### Lines of code 

- **All Contracts**

### Recommended Mitigation Steps

Line coverage percentage should be 100%.

## [NC-06] Function writing does not comply with the `Solidity Style Guide`

Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

Functions should be grouped according to their visibility and ordered:

- `constructor()`
- `receive()`  
- `fallback()`  
- `external` / `public` / `internal` / `private`
- `view` / `pure`

### Recommended Mitigation Steps

Follow Solidity Style Guide.

## [NC-07] Solidity compiler optimizations can be problematic

Protocol has enabled optional compiler optimizations in Solidity. There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them.

Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG.

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported. A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe. It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

Exploit Scenario A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.

```solidity
  solidity: {
    version: "0.8.8",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
```

### Lines of code

- [v2-token/hardhat.config.ts:33-34](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/hardhat.config.ts#L33-L34)
- [v2-pool/hardhat.config.ts:33-34](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/hardhat.config.ts#L33-L34)
- [hardhat.config.ts:34-35](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/hardhat.config.ts#L34-L35)

### Recommended Mitigation Steps

Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug. Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## [NC-08] Add a timelock to critical functions

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate.

```solidity
    /// @inheritdoc IOwnableTwoSteps
    function setPendingOwner(address chosenPendingOwner) external override {
        Ownership.checkIfOwner(owner);

        if (chosenPendingOwner == address(0)) Error.zeroAddress();
        chosenPendingOwner.checkIfAlreadyOwner(owner);

        pendingOwner = chosenPendingOwner;

        emit SetOwner(pendingOwner);
    }
```

### Lines of code

- [OwnableTwoSteps.sol:23](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L23)

### Recommended Mitigation Steps

Consider adding a timelock to the critical changes.

## [NC-09] Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length.

> Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

### Lines of code

- [FullMath.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol)
- [StrikeConversion.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol)
- [TimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol)
- [ITimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol)
- [Option.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol)
- [Process.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol)
- [Param.so](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol)
- [TimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol)
- [TimeswapV2PoolDeployer.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol)
- [ITimeswapV2Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol)
- [ITimeswapV2PoolBurnCallback.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol)
- [ITimeswapV2PoolDeleverageCallback.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol)
- [ITimeswapV2PoolLeverageCallback.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol)
- [ITimeswapV2PoolMintCallback.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol)
- [ConstantProduct.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol)
- [ConstantSum.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantSum.sol)
- [DurationCalculation.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/DurationCalculation.sol)
- [FeeCalculation.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol)
- [PoolFactory.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol)
- [LiquidityPosition.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol)
- [Param.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol)
- [Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol)
- [ERC1155Enumerable.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol)
- [ITimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol)
- [ITimeswapV2LiquidityTokenBurnCallback.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenBurnCallback.sol)
- [ITimeswapV2LiquidityTokenCollectCallback.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenCollectCallback.sol)
- [ITimeswapV2LiquidityTokenMintCallback.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol)
- [FeesPosition.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol)
- [TimeswapV2LiquidityToken.so](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol)
- [TimeswapV2Token.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol)

## [NC-10] NatSpec comments should be increased in contracts   

It is recommended that Solidity contracts are fully annotated using NatSpec, it is clearly stated in the Solidity official documentation.

- In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

- Some code analysis programs do analysis by reading NatSpec details, if they can't see the tags `(@param, @dev, @return)`, they do incomplete analysis.

### Lines of code

- **All Contracts**

### Recommended Mitigation Steps 

Include [`NatSpec`](https://docs.soliditylang.org/en/v0.8.15/natspec-format.html) comments in the codebase.

## [NC-11] Generate perfect code headers every time

I recommend using header for Solidity code layout and readability

> Ref: https://github.com/transmissions11/headers

### Lines of code 

- **All Contracts**

### Recommended Mitigation Steps

```
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
```

## [NC-12] For functions, follow Solidity standard naming conventions 

The protocol don't follow solidity standard naming convention.

> Ref: https://docs.soliditylang.org/en/v0.8.17/style-guide.html#naming-conventions

### Lines of code 

- **All Contracts**

### Recommended Mitigation Steps

Follow solidity standard naming convention.

## [NC-13] `Address(0)` checks  

Check of `address(0)` to protect the code from `(0x0000000000000000000000000000000000000000)` address problem just in case. This is best practice or instead of suggesting that they verify `_address != address(0)`, you could add some good NatSpec comments explaining what is valid and what is invalid and what are the implications of accidentally using an invalid address.

```solidity
    constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {
        optionFactory = chosenOptionFactory;
    }
```

### Lines of code 

- [TimeswapV2LiquidityToken.sol:36-39](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36-L39)
- [TimeswapV2Token.sol:41-43](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L41-L43)

### Recommended Mitigation Steps

```solidity
    constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {
        if (chosenOptionFactory == address(0)) revert();
        optionFactory = chosenOptionFactory;
    }
```

## [NC-14] Use a more recent version of solidity  

For security, it is best practice to use the latest Solidity version.

> https://github.com/ethereum/solidity/blob/develop/Changelog.md

### Lines of code 

- **All Contracts**

### Recommended Mitigation Steps

Old version of Solidity is used `(0.8.8)`, newer version can be used `(0.8.17)`

## [NC-15] Assembly Codes Specific – Should Have Comments     

Since this is a low level language that is more difficult to parse by readers, include extensive documentation, comments on the rationale behind its use, clearly explaining what each assembly instruction does

This will make it easier for users to trust the code, for reviewers to validate the code, and for developers to build on or update the code.

Note that using Aseembly removes several important security features of Solidity, which can make the code more insecure and more error-prone.

### Lines of code

- [FullMath.sol:47](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L47)
- [FullMath.sol:63](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L63)
- [FullMath.sol:78](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L78)
- [FullMath.sol:92](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L92)
- [FullMath.sol:103](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L103)

### Recommended Mitigation Steps

Include NatSpec in assembly codes.

## [NC-16] Events that mark critical parameter changes should contain both the old and the new value

```solidity
    event AcceptOwner(address owner);
```

```solidity
    function acceptOwner() external override {
        msg.sender.checkIfPendingOwner(pendingOwner);

        owner = msg.sender;
        delete pendingOwner;

        emit AcceptOwner(msg.sender);
    }
```

### Lines of code 

- [OwnableTwoSteps.sol:35-42](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L35-L42)
- [IOwnableTwoSteps.sol:11](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol#L11)

### Recommended Mitigation Steps

```solidity
    event AcceptOwner(address previousOwner, address newOwner);
```

```solidity
    function acceptOwner() external override {
        msg.sender.checkIfPendingOwner(pendingOwner);

        owner = msg.sender;
        delete pendingOwner;

        emit AcceptOwner(owner, msg.sender);
    }
```

## [NC-17] Consider using `delete` rather than assigning zero to clear values

The `delete` keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic

### Lines of code 

- [LiquidityPosition.sol:93](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L93)
- [LiquidityPosition.sol:101](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L101)
- [LiquidityPosition.sol:109](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L109)
- [Pool.sol:228](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L228)
- [Pool.sol:236](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L236)
- [Pool.sol:244](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L244)
- [Pool.sol:633](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L633)
- [Pool.sol:646](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L646)
- [Pool.sol:685](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L685)
- [Pool.sol:706](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L706)

### Recommended Mitigation Steps

Use the `delete` keyword.

## [NC-18] Add NatSpec Mapping comment     

Add NatSpec comments describing mapping keys and values

### Lines of code 

- [TimeswapV2Option.sol:53](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L53)
- [TimeswapV2OptionFactory.sol:22](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L22)
- [TimeswapV2Pool.sol:52](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L52)
- [TimeswapV2Pool.sol:53](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L53)
- [TimeswapV2PoolFactory.sol:31](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L31)
- [TimeswapV2LiquidityToken.sol:41](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L41)
- [TimeswapV2LiquidityToken.sol:43](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L43)
- [TimeswapV2LiquidityToken.sol:45](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L45)
- [TimeswapV2LiquidityToken.sol:47](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L47)
- [TimeswapV2Token.sol:36](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L36)
- [TimeswapV2Token.sol:38](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L38)
- [TimeswapV2Token.sol:39](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L39)
- [ERC1155Enumerable.sol:22](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L22)

## [NC-19] Using vulnerable dependency of OpenZeppelin 

The package.json configuration file says that the project is using 4.7.2 of OZ which has a not last update version.

```solidity
    "@openzeppelin/contracts": "^4.7.2",
```

### Lines of code

- [v2-token/package.json:40](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/package.json#L40)
- [v2-library/package.json:40](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/package.json#L40)

### Recommended Mitigation Steps

Use patched versions Latest non vulnerable version 4.8.0.

## [NC-20] Constants should be defined rather than using magic numbers 

### Lines of code 

- [BytesLib.sol:13](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L13)
- [BytesLib.sol:33](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L33)
- [CatchError.sol:15](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol#L15)
- [Transaction.sol:53](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol#L53)
- [Transaction.sol:58](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol#L58)
- [Transaction.sol:63](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol#L63)
- [Transaction.sol:68](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol#L68)
- [Transaction.sol:73](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol#L73)
- [ConstantProduct.sol:30](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L30)
- [ConstantProduct.sol:39](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L39)
- [ConstantProduct.sol:260](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L260)
- [ConstantProduct.sol:269](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L269)
- [ConstantProduct.sol:280](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L280)
- [ConstantProduct.sol:316](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L316)
- [ConstantProduct.sol#L332](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L332)
- [ConstantProduct.sol#L344](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L344)
- [ConstantProduct.sol:404](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L404)
- [ConstantProduct.sol:406](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L406)
