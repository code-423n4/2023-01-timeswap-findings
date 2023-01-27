## Redundant check 
In `transferLiquidity()` of TimeswapV2Pool.sol, `pool.liquidity != 0` will be checked in the external [`transferLiquidity()`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L160).

As such, invoking `hasLiquidity(strike, maturity)` preliminarily is unnecessary although this will make the call revert earlier if `pool.liquidity == 0`.  

(Note: A zero value check for `strike` and `maturity` is optionally included to replace `hasLiquidity()`, which is still going to cheaper in terns of gas savings. This is because the former is an inline code whereas the latter is a private function call.) 

Consider having the function refactored as follows to save gas on contract size and all successful calls:

[File: TimeswapV2Pool.sol#L152-L162](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L152-L162)

```diff
    function transferLiquidity(uint256 strike, uint256 maturity, address to, uint160 liquidityAmount) external override {
-        hasLiquidity(strike, maturity);
+        if (strike == 0 || maturity == 0) Error.cannotBeZero();

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
## Unusable functions and arrays
`create()` in TimeswapV2PoolFactory.sol and TimeswapV2OptionFactory.sol does not have `pair` and `optionPair` respectively pushed into `getByIndex`. For this reason, calling `numberOfPairs()` is going to always return `getByIndex.length == 0`.

Consider removing these unusable functions and state arrays to save gas on contract size:

[File: TimeswapV2PoolFactory.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol)

```diff
- 33:    address[] public override getByIndex;

- 52:    function numberOfPairs() external view override returns (uint256) {
- 53:        return getByIndex.length;
- 54:    }
```
[File: TimeswapV2OptionFactory.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol)

```diff
- 25:    address[] public override getByIndex;

- 36:    function numberOfPairs() external view override returns (uint256) {
- 37:        return getByIndex.length;
- 38:    }
```
## Inline code
Embedded function call entailing only one line of code may be made inline to save gas. 

For instance, in `create()` of TimeswapV2OptionFactory.sol, `OptionPairLibrary.checkDoesNotExist(token0, token1, optionPair)` is only making sure `optionPair == address(0)`.

For this reason, consider having the affected code line refactored as follows:

Note: The suggested error may have to be added/implemented in [Error.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol).

[File: TimeswapV2OptionFactory.sol#L43-L56](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L43-L56)

```diff
    function create(address token0, address token1) external override returns (address optionPair) {
        if (token0 == address(0)) Error.zeroAddress();
        if (token1 == address(0)) Error.zeroAddress();
        OptionPairLibrary.checkCorrectFormat(token0, token1);

        optionPair = optionPairs[token0][token1];
-        OptionPairLibrary.checkDoesNotExist(token0, token1, optionPair);
+        if (optionPair != address(0)) Error.alreadyExisted();

        optionPair = deploy(address(this), token0, token1);

        optionPairs[token0][token1] = optionPair;

        emit Create(msg.sender, token0, token1, optionPair);
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
## Function order affects gas consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
solidity: {
version: "0.8.13",

settings: {
  optimizer: {
    enabled: true,
    runs: 1000,
  },
},
},
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

```
for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI
```
Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## Use of named returns for local variables saves gas
You can have further advantages in terms of gas cost by simply using named return values as temporary local variable.

For instance, the code block below may be refactored as follows:

[File: TimeswapV2LiquidityToken.sol#L255-L257](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L255-L257)

```diff
-    function _additionalConditionAddTokenToOwnerEnumeration(address to, uint256 id) internal view override returns (bool) {
+    function _additionalConditionAddTokenToOwnerEnumeration(address to, uint256 id) internal view override returns (bool status_) {
-        return _additionalConditionForOwnerTokenEnumeration(to, id);
+        status_ = _additionalConditionForOwnerTokenEnumeration(to, id);
    }
```
