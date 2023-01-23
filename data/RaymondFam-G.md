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

