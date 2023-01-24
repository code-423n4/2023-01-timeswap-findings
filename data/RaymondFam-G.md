## Redundant check 
In `transferLiquidity()` of TimeswapV2Pool.sol, `pool.liquidity != 0` will be checked in the external [`transferLiquidity()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L160).

As such, invoking `hasLiquidity(strike, maturity)` preliminarily is unnecessary although this will make the call revert early if `pool.liquidity == 0`.  

Consider having the function refactored as follows to save gas on contract size and all successful calls:

[File: TimeswapV2Pool.sol#L152-L162](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L152-L162)

```diff
    function transferLiquidity(uint256 strike, uint256 maturity, address to, uint160 liquidityAmount) external override {
-        hasLiquidity(strike, maturity);

        if (blockTimestamp(0) > maturity) Error.alreadyMatured(maturity, blockTimestamp(0));
        if (to == address(0)) Error.zeroAddress();
        if (liquidityAmount == 0) Error.zeroInput();

        pools[strike][maturity].transferLiquidity(to, liquidityAmount, blockTimestamp(0));

        emit TransferLiquidity(strike, maturity, msg.sender, to, liquidityAmount);
    }
```
## Unneeded if block
In `updateDurationWeightBeforeMaturity()` of Pool.sol, blockTimestamp == [blockTimestamp(0)](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L159) and blockTimestamp(0) == [block.timestamp](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L82-L84). As such, `pool.lastTimestamp < blockTimestamp` will always hold true.

Consider having the if block removed to save gas both on contract deployment and function calls:

[File: Pool.sol#L128-L137](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L128-L137)

```diff
    function updateDurationWeightBeforeMaturity(Pool storage pool, uint96 blockTimestamp) private {
-        if (pool.lastTimestamp < blockTimestamp)
            (pool.lastTimestamp, pool.shortFeeGrowth) = updateDurationWeight(
                pool.liquidity,
                pool.sqrtInterestRate,
                pool.shortFeeGrowth,
                DurationCalculation.unsafeDurationFromLastTimestampToNow(pool.lastTimestamp, blockTimestamp),
                blockTimestamp
            );
    }
```
## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =).

As an example, consider replacing `>=` with the strict counterpart `>` in the following inequality instance:

[File: BytesLib.sol#L13](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L13)

```diff
-        require(_length + 31 >= _length, "slice_overflow");
// Rationale for subtracting 1 on the right side of the inequality:
// x >= 10; [10, 11, 12, ...]
// x > 10 - 1 is the same as x > 9; [10, 11, 12 ...] 
+        require(_length + 31 > _length - 1, "slice_overflow");
```
Similarly, the `<=` instance below may be replaced with `<` as follows:

[File: FullMath.sol#L189](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L189)

```diff
-        if (divisor <= product1) revert MulDivOverflow(multiplicand, multiplier, divisor);
// Rationale for adding 1 on the right side of the inequality:
// x <= 10; [10, 9, 8, ...]
// x < 10 + 1 is the same as x < 11; [10, 9, 8 ...]
+        if (divisor < product1 + 1) revert MulDivOverflow(multiplicand, multiplier, divisor);
```
## += and -= cost more gas
`+=` and `-=` generally cost 22 more gas than writing out the assigned equation explicitly. The amount of gas wasted can be quite sizable when repeatedly operated in a loop.

For instance, the `+=` instance below may be refactored as follows:

[File: FullMath.sol#L163](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L163)

```diff
-            productA1 += (quotient1 * divisor);
+            productA1 = productA1 + (quotient1 * divisor);
```
## `||` costs less gas than its equivalent `&&`
Rule of thumb: `(x && y)` is `(!(!x || !y))`

Even with the 10k Optimizer enabled: `||`, OR conditions cost less than their equivalent `&&`, AND conditions.

As an example, the `&&` instance below may be refactored as follows:

Note: Comment on the changes made for better readability where deemed fit.

[File: Math.sol#L51](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L51)

```diff
-        if (roundUp && dividend % divisor != 0) quotient++;
+        if (!(!roundUp || dividend % divisor == 0)) quotient++;
```
## Ternary over `if ... else`
Using ternary operator instead of the if else statement saves gas.

For instance, the specific instance below may be refactored as follows:

[File: ConstantProduct.sol#L149-L150](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L149-L150)

```diff
-        if (isAdd) longAmount -= fees;
-        else shortAmount -= fees;

+      isAdd
+          ? longAmount -= fees
+          : shortAmount -= fees;
```
## Use storage instead of memory for structs/arrays
A storage pointer is cheaper since copying a state struct in memory would incur as many SLOADs and MSTOREs as there are slots. In another words, this causes all fields of the struct/array to be read from storage, incurring a Gcoldsload (2100 gas) for each field of the struct/array, and then further incurring an additional MLOAD rather than a cheap stack read. As such, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables will be much cheaper, involving only Gcoldsload for all associated field reads. Read the whole struct/array into a memory variable only when it is being returned by the function, passed into a function that requires memory, or if the array/struct is being read from another memory array/struct.

For instance, the specific instance below may be refactored as follows:

[File: Token.sol#L275](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L275)

```diff
-        FeesPosition memory feesPosition = _feesPositions[id][owner];
+        FeesPosition storage feesPosition = _feesPositions[id][owner];
```
## Unchecked SafeMath saves gas
"Checked" math, which is default in ^0.8.0 is not free. The compiler will add some overflow checks, somehow similar to those implemented by `SafeMath`. While it is reasonable to expect these checks to be less expensive than the current `SafeMath`, one should keep in mind that these checks will increase the cost of "basic math operation" that were not previously covered. This particularly concerns variable increments in for loops. When no arithmetic overflow/underflow is going to happen, `unchecked { ... }` to use the previous wrapping behavior further saves gas.

For instance, the code b;lock below may be refactored as follows:

[File: FeesPosition.sol#L52-L56](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L52-L56)

```diff
    function mint(FeesPosition storage feesPosition, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) internal {
+      unchecked {
        feesPosition.long0Fees += long0Fees;
        feesPosition.long1Fees += long1Fees;
        feesPosition.shortFees += shortFees;
+      }
    }
```