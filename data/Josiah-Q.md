## OBSOLETE FUNCTIONS AND VARIABLES
In [TimeswapV2PoolFactory.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L59-L70) and [TimeswapV2OptionFactory.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L43-L56), it is noted that their corresponding `pair` and `optionPair` have not been pushed into the array, `getByIndex` in the function logic of `create()`. This leads to futile `numberOfPairs()` that is going to always return `getByIndex.length == 0`.

Suggested fix:

It is recommended pushing these useful elements, `pair` and `optionPair`, into their respective `getByIndex` when calling `create()`.

## CONFUSING RETURN STATEMENT
Using `return` to output function results is a confusing approach in the presence of named returns.

Here is a specific instance found.

[Math.sol#L88-L90](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L88-L90)

```
    function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {
        return value1 < value2 ? value1 : value2;
    }
```
Suggested fix:

- Remove `return`

```
    function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {
        result = value1 < value2 ? value1 : value2;
    }
```
or

.  Remove `result` 

```
    function min(uint256 value1, uint256 value2) internal pure returns (uint256) {
        return value1 < value2 ? value1 : value2;
    }
```
## CONFUSING ERROR UTILIZED
In TimeswapV2PoolFactory.sol, the second if check of `create()` reverts via `Error.zeroAddress()` if `pair` is not a zero address. This is confusing the users because the error thrown applies more appropriately the opposite way if `pair == address(0)`. 

[TimeswapV2PoolFactory.sol#L63](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63)

```
        if (pair != address(0)) Error.zeroAddress();
```
Suggested fix:

Implement something meaningful, e.g. `Error.addressNotZero` or `Error.duplicatePair` and update it in Error.sol. 

## ONE SIDED CONDITION
In Pool.sol, the condition in the if statement of `updateDurationWeightBeforeMaturity()`, `pool.lastTimestamp < blockTimestamp` will always be true, making the check unnecessary.

[Pool.sol#L129](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L129) 

```
    function updateDurationWeightBeforeMaturity(Pool storage pool, uint96 blockTimestamp) private {
        if (pool.lastTimestamp < blockTimestamp)
            (pool.lastTimestamp, pool.shortFeeGrowth) = updateDurationWeight(
                pool.liquidity,
                pool.sqrtInterestRate,
                pool.shortFeeGrowth,
                DurationCalculation.unsafeDurationFromLastTimestampToNow(pool.lastTimestamp, blockTimestamp),
                blockTimestamp
            );
```
Suggested fix:

Remove the if check and proceed to executing the associated block logic.  

## NATSPEC ACCURACY
Comments in the function NatSpec should be as accurate as possible to avoid confusing the users.

For example, as denoted in the whitepaper:

"Let I be the marginal interest rate per second of the Short per total Long."

However, [the second line of `sqrtInterestRate()` NatSpec](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L170) says that:

"... the square root of interest rate is z/(x+y) ..."

Suggested fix:

Update the function NatSpec to correctly portray its intended message.