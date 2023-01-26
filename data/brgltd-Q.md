# [01] Lack of support for rebasing/deflationary/inflationary tokens

The protocol does not appear to support rebasing/deflationary/inflationary tokens whose balance changes during transfers or over time. 

The necessary checks include at least verifying the amount of tokens transferred to contracts before and after the actual transfer to infer any fees/interest.

## Proof of Concept

Executing transfers and computing internal booking state variables with the same values.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L171-L175

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L190-L191

## Recommended Mitigation Steps

Add the necessary calculations to use rebasing/deflationary/inflationary tokens. If these tokens are not intended to be supported by the protocol, consider adding comments on the project for clarification.

# [02] Lack of SafeCast inside `Pool.sol`

Most casting operations in the project are using the `SafeCast.sol` library, with the exception of `Pool.sol`. 

Consider using `SafeCast.sol` inside `Pool.sol`. Otherwise, the protocol can be subject of silent overflows resulting in accounting issues.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L249

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L251

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L277

# [03] Frontrun `TimewapV2Pool.initialize()`

It's possible for an attacker to call `initialize()` before the contract owner.

In the best case, the owner is forced to waste gas and re-deploy. In the worst case, the owner does not notice that his/her call reverts, and everyone starts using a contract under the control of an attacker.

Consider adding access control on `initialize()` so that it can only be called by the contract owner (that address that executed the constructor code) or trusted addresses.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L175-L181

# [04] Lack of reentrancy guard and checks-effects-interactions for `TimeswapV2Option` `burn()` and `collect()` functions

The functions `burn()` and `collect()` for `TimeswapV2Option.sol` are open to reentrancy. The comment on [Process.sol L27](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L27) mentions reentrancy protection for `process`. However the `option` state variable inside `TimeswapV2Option.sol` is updated after the external transfers.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L172-L175

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L190-L192

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L258-L262

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L277

## Recommendation

Consider adding a reentrancy guard on `burn()` and `collect()`. Also, consider making state updates before external calls.

# [05] Avoid pragma float

Locking the pragma helps to ensure that contracts do not accidentally get deployed using an outdated compiler version.

Note that pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or a package.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L2

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L4

# [06] Use a more recent version of solidity

Using the fixed latest stable version (v.0.8.17) helps to ensure the compiler has the latest security fixes and the latest features, e.g. for versions >=0.8.10, external calls skip contract existence checks if the external call has a return value.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L2

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L2

# [07] Emit events before external calls

Multiple functions in the project emit an event as the last statement. Wherever possible, consider emitting events before external calls. In case of reentrancy, funds are not at risk (for external call + event ordering), however emitting events after external calls can damage frontends and monitoring tools in case of reentrancy attacks.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L377-L418

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L430-L466

# [08] Unbounded loop for state/storage variables

Iterating state/storage variables from 0 to length -1 can run out of gas if the state/storage variable has too many items. Wherever possible, consider adding a slice functionality on the functions calling `Process.updateProcess()`, so that `Process.updateProcess()` can be called in chunks and accepts a `startIndex` and an `endIndex` variables.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L33-L45

# [09] Lack of address(0) check for the function argument `to` in `Pool.transferLiquidity()`

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L158

# [10] Unclaimed packages

The following packages used by the system are not published on npm

https://www.npmjs.com/search?q=@timeswap-labs/v2-option
https://www.npmjs.com/search?q=@timeswap-labs/v2-library
https://www.npmjs.com/search?q=@timeswap-labs/v2-library

A vulnerability related to this on the last Timeswap [contest](https://code4rena.com/reports/2022-03-timeswap#m-03-npm-dependency-confusion-unclaimed-npm-package-and-scopeorg).

It does not seem to be a vulnerability at this point, since npm doesn't allow for different organizations to have the same name.

However, I would still recommend to publish the mentioned packages on npm. E.g. is npm ever allows for different companies to register the same name, an attacker could upload malicious code for @timeswap-labs/v2-option and execute malicious code on users machines. 

# [11] Unused return named variable

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L88-L90

# [12] Missing NATSPEC

Consider adding NATSPEC on all external/public functions to improve documentation.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L246

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L252

# [13] Order of functions

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends the following order for functions:

constructor
external
public
internal
private

The instance bellow shows an different ordering being used in `TimeswapV2Pool.sol`.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L71-L91

# [14] Add a limit for the maximum number of characters per line

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) recommends a maximum of 120 characters.

Consider adding a limit of 120 characters or less to prevent large lines.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L246

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L32

# [15] Remove console.log

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L109

# [16] Repeated validations statements

Repeated validation statements can be converted into a function modifier to improve code reusability.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L156

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L166
