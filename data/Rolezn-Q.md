## Summary<a name="Summary">

### Low Risk Issues
| |Issue|Contexts|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | Low Level Calls With Solidity Version 0.8.14 Can Result In Optimiser Bug | 1 |
| [LOW&#x2011;2](#LOW&#x2011;2) | Use `_safeMint` instead of `_mint` | 4 |
| [LOW&#x2011;3](#LOW&#x2011;3) | Missing parameter validation in `constructor` | 5 |
| [LOW&#x2011;4](#LOW&#x2011;4) | Contracts are not using their OZ Upgradeable counterparts | 9 |
| [LOW&#x2011;5](#LOW&#x2011;5) | Remove unused code | 3 |

Total: 22 contexts over 5 issues

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Add a timelock to critical functions | 1 |
| [NC&#x2011;2](#NC&#x2011;2) | Avoid Floating Pragmas: The Version Should Be Locked | 13 |
| [NC&#x2011;3](#NC&#x2011;3) | Critical Changes Should Use Two-step Procedure | 1 |
| [NC&#x2011;4](#NC&#x2011;4) | Function writing that does not comply with the Solidity Style Guide  | 88 |
| [NC&#x2011;5](#NC&#x2011;5) | Use `delete` to Clear Variables | 11 |
| [NC&#x2011;6](#NC&#x2011;6) | NatSpec return parameters should be included in contracts | All in-scope contracts |
| [NC&#x2011;7](#NC&#x2011;7) | Lines are too long | 10 |
| [NC&#x2011;8](#NC&#x2011;8) | Implementation contract may not be initialized | 8 |
| [NC&#x2011;9](#NC&#x2011;9) | NatSpec comments should be increased in contracts | All in-scope contracts |
| [NC&#x2011;10](#NC&#x2011;10) | Use a more recent version of Solidity | 88 |

Total: 396 contexts over 10 issues

## Low Risk Issues





### <a href="#Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> Low Level Calls With Solidity Version Before 0.8.14 Can Result In Optimiser Bug

The project contracts in scope are using low level calls with solidity version before 0.8.14 which can result in optimizer bug.
https://medium.com/certora/overly-optimistic-optimizer-certora-bug-disclosure-2101e3f7994d

Simliar findings in Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#5-low-level-calls-with-solidity-version-0814-can-result-in-optimiser-bug

#### <ins>Proof Of Concept</ins>

POC can be found in the above medium reference url.

Functions that execute low level calls in contracts with solidity version under 0.8.14

```solidity
18: assembly {
            switch iszero(_length)
            case 0 {
                // Get a location of some free memory and store it in tempBytes as
                // Solidity does for memory variables.
                tempBytes := mload(0x40)

                // The first word of the slice result is potentially a partial
                // word read from the original array. To read it, we calculate
                // the length of that partial word and start copying that many
                // bytes into the array. The first word we copy will start with
                // data we don't care about, but the last `lengthmod` bytes will
                // land at the beginning of the contents of the new array. When
                // we're done copying, we overwrite the full first word with
                // the actual length of the slice.
                let lengthmod := and(_length, 31)

                // The multiplication in the next line is necessary
                // because when slicing multiples of 32 bytes (lengthmod == 0)
                // the following copy loop was copying the origin's length
                // and then ending prematurely not copying everything it should.
                let mc := add(add(tempBytes, lengthmod), mul(0x20, iszero(lengthmod)))
                let end := add(mc, _length)

                for {
                    // The multiplication in the next line has the same exact purpose
                    // as the one above.
                    let cc := add(add(add(_bytes, lengthmod), mul(0x20, iszero(lengthmod))), _start)
                } lt(mc, end) {
                    mc := add(mc, 0x20)
                    cc := add(cc, 0x20)
                } {
                    mstore(mc, mload(cc))
                }

                mstore(tempBytes, _length)

                //update free-memory pointer
                //allocating the array padded to 32 bytes like the compiler does now
                mstore(0x40, and(add(mc, 31), not(31)))
            }
            //if we want a zero-length slice let's just return a zero-length array
            default {
                tempBytes := mload(0x40)
                //zero out the 32 bytes slice we are about to return
                //we need to do it because Solidity does not garbage collect
                mstore(tempBytes, 0)

                mstore(0x40, add(tempBytes, 0x20))
            }
        }
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/BytesLib.sol#L18





#### <ins>Recommended Mitigation Steps</ins>

Consider upgrading to at least solidity v0.8.15.



### <a href="#Summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> Use `_safeMint` instead of `_mint`

According to openzepplin's ERC721, the use of `_mint` is discouraged, use _safeMint whenever possible.
https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-_mint-address-uint256-

#### <ins>Proof Of Concept</ins>


```solidity
128: _mint(param.to, id, param.liquidityAmount, bytes(""));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L128

```solidity
110: _mint(param.long0To, id, (param.long0Amount), bytes(""));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L110

```solidity
139: _mint(param.long1To, id, (param.long1Amount), bytes(""));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L139

```solidity
168: _mint(param.shortTo, id, (param.shortAmount), bytes(""));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L168



#### <ins>Recommended Mitigation Steps</ins>

Use `_safeMint` whenever possible instead of `_mint`



### <a href="#Summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> Missing parameter validation in `constructor`

Some parameters of constructors are not checked for invalid values.

#### <ins>Proof Of Concept</ins>

```solidity
37: address chosenOwner

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37

```solidity
18: address chosenOwner

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L18

```solidity
36: address chosenOptionFactory
36: address chosenPoolFactory

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36-L36

```solidity
41: address chosenOptionFactory

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L41



#### <ins>Recommended Mitigation Steps</ins>

Validate the parameters.



### <a href="#Summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> Contracts are not using their OZ Upgradeable counterparts

The non-upgradeable standard version of OpenZeppelin’s library are inherited / used by the contracts.
It would be safer to use the upgradeable versions of the library contracts to avoid unexpected behaviour.

#### <ins>Proof of Concept</ins>

```solidity
4: import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
5: import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L4-L5

```solidity
4: import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
5: import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L4-L5

```solidity
4: import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L4

```solidity
4: import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L4

```solidity
6: import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L6

```solidity
6: import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L6

```solidity
4: import {IERC1155} from "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L4



#### <ins>Recommended Mitigation Steps</ins>

Where applicable, use the contracts from `@openzeppelin/contracts-upgradeable` instead of `@openzeppelin/contracts`.




### <a href="#Summary">[LOW&#x2011;5]</a><a name="LOW&#x2011;5"> Remove unused code
This code is not used in the main project contract files, remove it or add event-emit
Code that is not in use, suggests that they should not be present and could potentially contain insecure functionalities.

#### <ins>Proof Of Concept</ins>






```solidity
function invalidTransaction
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/enums/Transaction.sol#L47


```solidity
function addFees
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L47

```solidity
function _afterTokenTransfer
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L81






## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Add a timelock to critical functions

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user). Consider adding a timelock to the following functions:

#### <ins>Proof Of Concept</ins>


```solidity
23: function setPendingOwner(address chosenPendingOwner) external override {
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L23







### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Avoid Floating Pragmas: The Version Should Be Locked

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### <ins>Proof Of Concept</ins>

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [TimeswapV2LiquidityToken.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [TimeswapV2Token.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L2

```solidity
Found usage of floating pragmas ^0.8.0 of Solidity in [ERC1155Enumerable.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L4

```solidity
Found usage of floating pragmas ^0.8.0 of Solidity in [IERC1155Enumerable.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L4

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [CallbackParam.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [CallbackParam.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [CallbackParam.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [FeesPosition.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/FeesPosition.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [Param.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [Param.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [Param.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [Position.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L2

```solidity
Found usage of floating pragmas ^0.8.8 of Solidity in [Position.sol]
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L2








### <a href="#Summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

#### <ins>Proof Of Concept</ins>

```solidity
23: function setPendingOwner(address chosenPendingOwner) external override {
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L23



#### <ins>Recommended Mitigation Steps</ins>

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.





### <a href="#Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> Function writing that does not comply with the Solidity Style Guide

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
- within a grouping, place the view and pure functions last

#### <ins>Proof Of Concept</ins>

All in-scope contracts




### <a href="#Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Use `delete` to Clear Variables

`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at `x`.

The `delete` key better conveys the intention and is also more idiomatic. Consider replacing assignments of zero with `delete` statements.

#### <ins>Proof Of Concept</ins>

```solidity
166: quotient0 = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/FullMath.sol#L166

```solidity
93: liquidityPosition.long0Fees = 0;
101: liquidityPosition.long1Fees = 0;
109: liquidityPosition.shortFees = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L93

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L101

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L109



```solidity
228: pool.long0ProtocolFees = 0;
236: pool.long1ProtocolFees = 0;
244: pool.shortProtocolFees = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L228

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L236

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L244



```solidity
633: pool.long0Balance = 0;
646: pool.long1Balance = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L633

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L646



```solidity
685: pool.long1Balance = 0;
706: pool.long0Balance = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L685

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L706







### <a href="#Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> NatSpec return parameters should be included in contracts

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html
#### <ins>Proof Of Concept</ins>


All in-scope contracts


#### <ins>Recommended Mitigation Steps</ins>

Include return parameters in NatSpec comments

Recommendation Code Style: (from Uniswap3)

```solidity
    /// @notice Adds liquidity for the given recipient/tickLower/tickUpper position
    /// @dev The caller of this method receives a callback in the form of IUniswapV3MintCallback#uniswapV3MintCallback
    /// in which they must pay any token0 or token1 owed for the liquidity. The amount of token0/token1 due depends
    /// on tickLower, tickUpper, the amount of liquidity, and the current price.
    /// @param recipient The address for which the liquidity will be created
    /// @param tickLower The lower tick of the position in which to add liquidity
    /// @param tickUpper The upper tick of the position in which to add liquidity
    /// @param amount The amount of liquidity to mint
    /// @param data Any data that should be passed through to the callback
    /// @return amount0 The amount of token0 that was paid to mint the given amount of liquidity. Matches the value in the callback
    /// @return amount1 The amount of token1 that was paid to mint the given amount of liquidity. Matches the value in the callback
    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);
```





### <a href="#Summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length
Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

#### <ins>Proof Of Concept</ins>

```solidity
62: /// @param amount If isLong0ToLong1 and transaction is GivenToken0AndLong0, this is the amount of token0 withdrawn, and the amount of long0 position burnt.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L62

```solidity
63: /// If isLong1ToLong0 and transaction is GivenToken0AndLong0, this is the amount of token0 to be deposited, and the amount of long0 position minted.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L63

```solidity
64: /// If isLong0ToLong1 and transaction is GivenToken1AndLong1, this is the amount of token1 to be deposited, and the amount of long1 position minted.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L64

```solidity
62: /// @param amount If isLong0ToLong1 and transaction is GivenToken0AndLong0, this is the amount of token0 withdrawn, and the amount of long0 position burnt.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L62

```solidity
63: /// If isLong1ToLong0 and transaction is GivenToken0AndLong0, this is the amount of token0 to be deposited, and the amount of long0 position minted.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L63

```solidity
64: /// If isLong0ToLong1 and transaction is GivenToken1AndLong1, this is the amount of token1 to be deposited, and the amount of long1 position minted.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L64

```solidity
62: /// @param amount If isLong0ToLong1 and transaction is GivenToken0AndLong0, this is the amount of token0 withdrawn, and the amount of long0 position burnt.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L62

```solidity
63: /// If isLong1ToLong0 and transaction is GivenToken0AndLong0, this is the amount of token0 to be deposited, and the amount of long0 position minted.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L63

```solidity
64: /// If isLong0ToLong1 and transaction is GivenToken1AndLong1, this is the amount of token1 to be deposited, and the amount of long1 position minted.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L64

```solidity
87: /// @dev Calculate the amount of liquidity positions and amount of long positions in base denomination or short position whichever is larger or smaller.
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L87








### <a href="#Summary">[NC&#x2011;8]</a><a name="NC&#x2011;8"> Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

#### <ins>Proof Of Concept</ins>


```solidity
19: constructor()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/NoDelegateCall.sol#L19

```solidity
65: constructor() NoDelegateCall()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L65


```solidity
77: constructor() NoDelegateCall()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L77

```solidity
37: constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner)
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37

```solidity
18: constructor(address chosenOwner)
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L18

```solidity
36: constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address")
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36

```solidity
41: constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address")
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L41





### <a href="#Summary">[NC&#x2011;9]</a><a name="NC&#x2011;9"> NatSpec comments should be increased in contracts

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability. https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

#### <ins>Proof Of Concept</ins>


All in-scope contracts


#### <ins>Recommended Mitigation Steps</ins>

NatSpec comments should be increased in contracts



### <a href="#Summary">[NC&#x2011;10]</a><a name="NC&#x2011;10"> Use a more recent version of Solidity

<a href="https://blog.soliditylang.org/2021/04/21/solidity-0.8.4-release-announcement/">0.8.4</a>:
bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)

<a href="https://blog.soliditylang.org/2022/02/16/solidity-0.8.12-release-announcement/">0.8.12</a>: 
string.concat() instead of abi.encodePacked(<str>,<str>)

<a href="https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/">0.8.13</a>: 
Ability to use using for with a list of free functions

<a href="https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/">0.8.14</a>:

ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against calldatasize() in all cases.
Override Checker: Allow changing data location for parameters only when overriding external functions.

<a href="https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/">0.8.15</a>:

Code Generation: Avoid writing dirty bytes to storage when copying bytes arrays.
Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

<a href="https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/">0.8.16</a>:

Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

<a href="https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/">0.8.17</a>:

Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

#### <ins>Proof Of Concept</ins>


```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/BytesLib.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/CatchError.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Error.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/FullMath.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Math.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Ownership.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/SafeCast.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/StrikeConversion.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/NoDelegateCall.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/NoDelegateCall.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/enums/Position.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/enums/Position.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/enums/Transaction.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/enums/Transaction.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/libraries/OptionFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/libraries/OptionPair.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/libraries/Proportion.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Process.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/StrikeAndMaturity.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/NoDelegateCall.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/NoDelegateCall.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/enums/Transaction.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/enums/Transaction.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantSum.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/Duration.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/DurationCalculation.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/DurationWeight.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/Fee.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/PoolFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/PoolPair.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Param.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Param.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Param.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L2

```solidity
pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L4

```solidity
pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L4

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2TokenMintCallback.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/FeesPosition.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L2



#### <ins>Recommended Mitigation Steps</ins>

Consider updating to a more recent solidity version.



