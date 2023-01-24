## Typo/grammatical mistakes
[File: FullMath.sol#L1](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L1)

Note: The following correction represents only one of the numerous contract instances with the same grammatical error entailed.

```diff
- //SPDX-License-Identifier: Unlicense
+ //SPDX-License-Identifier: Unlicensed
```
[File: Math.sol#L84](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L84)

```diff
-    /// @dev Gets the min of two uint256 number.
+    /// @dev Gets the min of two uint256 numbers.
```
[File: ITimeswapV2PoolBurnCallback.sol#L8](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol#L8)
[File: ITimeswapV2PoolLeverageCallback.sol#L8](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L8)
[File: ITimeswapV2PoolMintCallback.sol#L8](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L8)
[File: ITimeswapV2PoolDeleverageCallback.sol#L8](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L8)

```diff
-    /// @dev Returns the amount of long0 position and long1 positions chosen to be withdrawn.
+    /// @dev Returns the amount of long0 position and long1 position chosen to be withdrawn.
```
[File: ITimeswapV2PoolRebalanceCallback.sol#L10](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol#L10)
[File: ITimeswapV2PoolLeverageCallback.sol#L10](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L10)
[File: ITimeswapV2PoolMintCallback.sol#L10](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L10)
[File: ITimeswapV2PoolDeleverageCallback.sol#L10](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L10)
[File: Param.sol#L11-L13](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L11-L13)
[File: Pool.sol#L168](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L168)

```diff
-    /// @dev The long0 positions or long1 positions will already be minted to the receipient.
+    /// @dev The long0 positions or long1 positions will already be minted to the recipient.
```
[File: OwnableTwoSteps.sol#L9](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L9)

```diff
- /// @dev contract for ownable implementation with a safety two step owner transfership.
+ /// @dev contract for ownable implementation with a safety two step ownership transfer.
```
[File: PoolFactory.sol#L41-L42](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L41-L42)

```diff
-    /// @return optionPair The retreived option pair address.
-    /// @return poolPair The retreived pool pair address.
+    /// @return optionPair The retrieved option pair address.
+    /// @return poolPair The retrieved pool pair address.
```
[File: ReentrancyGuard.sol#L11](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L11)

```diff
-    /// @dev The initial state which must be change to NOT_ENTERED when first interacting.
+    /// @dev The initial state which must be changed to NOT_ENTERED when first interacting.
```
## Minimization of truncation
The number of divisions in an equation should be reduced to minimize truncation frequency, serving to achieve higher precision. And, where deemed fit, comment the code line with the original multiple division arithmetic operation for clarity reason.

For instance, the code line below involving two divisions may be refactored as follows:

[File: Math.sol#L77](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L77)

```diff
-                estimate = (value / estimate + estimate) >> 1;
+                estimate = (value + estimate * estimate) / (estimate * 2);
```
## Lines too long
Lines in source code are typically limited to 80 characters, but it’s reasonable to stretch beyond this limit when need be as monitor screens theses days are comparatively larger. Considering the files will most likely reside in GitHub that will have a scroll bar automatically kick in when the length is over 164 characters, all code lines and comments should be split when/before hitting this length. Keep line width to max 120 characters for better readability where possible.

Here are some of the instances entailed:

[File: StrikeConversion.sol#L27](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol#L27)
[File: TimeswapV2Pool.sol#L31-L34](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L31-L34)
[File: TimeswapV2Pool.sol#L293-L297](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L293-L297)

## Inappropriate error thrown
The second if block of `create()` in TimeswapV2PoolFactory.sol throws `Error.zeroAddress()` if `pair != address(0)`, which is misleading. 

Consider having the affected code line refactored as follows to avoid any confusion to the users/developers:

Note: The suggested error may have to be added/implemented in [Error.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol).

[File: TimeswapV2PoolFactory.sol#L59-L70](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L59-L70)

```diff
    function create(address optionPair) external override returns (address pair) {
        if (optionPair == address(0)) Error.zeroAddress();

        pair = pairs[optionPair];
-        if (pair != address(0)) Error.zeroAddress();
+        if (pair != address(0)) Error.alreadyCreatedPool();

        pair = deploy(address(this), optionPair, transactionFee, protocolFee);

        pairs[optionPair] = pair;

        emit Create(msg.sender, optionPair, pair);
    }
```
## Solidity's Style Guide on contract layout
According to Solidity's Style Guide below:

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

In order to help readers identify which functions they can call, and find the constructor and fallback definitions more easily, functions should be grouped according to their visibility and ordered in the following manner:

constructor, receive function (if exists), fallback function (if exists), external, public, internal, private

And, within a grouping, place the view and pure functions last.

Additionally, inside each contract, library or interface, use the following order:

type declarations, state variables, events, modifiers, functions

Consider adhering to the above guidelines for all contract instances entailed.
 
## Inadequate NatSpec
Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

For instance, it will be of added values to the users and developers if:
- `@ return` is also provided on the functions of [ConstantSum.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantSum.sol).
- at least a minimalist NatSpec is provided on [FeesPosition.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol).

## Inappropriate comment
The return code line in the function below entails only a simple addition that is devoid of division. The comment, `truncation is desired` is therefore deemed non-matching. Consider removing/editing the comment where possible:

[File: TimeswapV2Pool.sol#L82-L84](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L82-L84) 

```solidity
    function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {
        return uint96(block.timestamp + durationForward); // truncation is desired
    }
```
## Erroneous function NatSpec
[File: ITimeswapV2PoolDeployer.sol#L11-L18](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol#L11-L18)

```diff
    /// @notice Get the parameters to be used in constructing the pair, set transiently during pair creation.
    /// @dev Called by the pair constructor to fetch the parameters of the pair.
    /// @return poolFactory The poolFactory address.
-    /// @param optionPair The Timeswap V2 OptionPair address.
+    /// @return optionPair The Timeswap V2 OptionPair address.
-    /// @param transactionFee The transaction fee earned by the liquidity providers.
+    /// @return transactionFee The transaction fee earned by the liquidity providers.
-    /// @param protocolFee The protocol fee earned by the DAO.
+    /// @return protocolFee The protocol fee earned by the DAO.
    function parameter() external view returns (address poolFactory, address optionPair, uint256 transactionFee, uint256 protocolFee);
}
```
## Missing zero value checks
When initializing the pool, a boundary and a zero value check has correspondingly been applied on `maturity` and `rate`.

A zero value check should likewise be executed on `strike` to make the input parameters of `initialize()` more error free:

[File: TimeswapV2Pool.sol#L175-L181](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L175-L181)

```diff
    function initialize(uint256 strike, uint256 maturity, uint160 rate) external override noDelegateCall {
        if (maturity < blockTimestamp(0)) Error.alreadyMatured(maturity, blockTimestamp(0));
-        if (rate == 0) Error.cannotBeZero();
+        if (rate == 0 || strike == 0) Error.cannotBeZero();
        addPoolEnumerationIfNecessary(strike, maturity);

        pools[strike][maturity].initialize(rate);
    }
```
The same check shall also apply on [`transferLiquidity()` and `transferFees()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L152-L172).

## Use `delete` to clear variables
`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address or a boolean to false. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at x.

The delete key better conveys the intention and is also more idiomatic.

For instance, the `a = 0` instance below may be refactored as follows:

[File: Pool.sol#L685](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L685)

```diff
-                    pool.long1Balance = 0;
+                    delete pool.long1Balance;
```
## Variable Assignment in Conditional Check
Making a variable assignment in a conditional statement deviates from the standard use and intention of the check and can easily lead to confusion.

Here are the instances entailed that should have the needed assignment cached before the conditional statement as follows:

[File: Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol)

```diff
- 681:                if ((long1Amount = param.delta) == long1AmountAdjustFees) {

+                long1Amount = param.delta);
+                if (long1Amount == long1AmountAdjustFees) {

- 702:                if ((long0Amount = param.delta) == long0AmountAdjustFees) {

+                long0Amount = param.delta);
+                if (long0Amount == long0AmountAdjustFees) {
```