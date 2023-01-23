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

```diff
-    /// @dev The long0 positions or long1 positions will already be minted to the receipient.
+    /// @dev The long0 positions or long1 positions will already be minted to the recipient.
```
[File: OwnableTwoSteps.sol#L9](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L9)

```diff
- /// @dev contract for ownable implementation with a safety two step owner transfership.
+ /// @dev contract for ownable implementation with a safety two step ownership transfer.
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