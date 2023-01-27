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

## TOKEN SORT LOGIC
In TimeswapV2OptionFactory.sol, `create()` will readily revert if `token0 > token1`, which is not very efficient in terms of gas efficiency and user friendliness.

[TimeswapV2OptionFactory.sol#L46](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L46)

```
        OptionPairLibrary.checkCorrectFormat(token0, token1);
```
Suggested fix:

Replace the above code line above with a sort logic that is only 1 code line long:

```
    function create(address _token0, address _token1) external override returns (address optionPair) {
        if (_token0 == address(0)) Error.zeroAddress();
        if (_token1 == address(0)) Error.zeroAddress();
        (token0, token1) = token0_ < token1_ ? (token0_, token1_) : (token1_, token0_);

        optionPair = optionPairs[token0][token1];
        OptionPairLibrary.checkDoesNotExist(token0, token1, optionPair);

        optionPair = deploy(address(this), token0, token1);

        optionPairs[token0][token1] = optionPair;

        emit Create(msg.sender, token0, token1, optionPair);
    }
```
## A SIMPLE ZERO ADDRESS CHECK
In TimeswapV2OptionFactory.sol, `OptionPairLibrary.checkDoesNotExist()` in `create()` is simply making sure `optionPair` is a zero address. There isn't really a need to call the library for this simple check.

[TimeswapV2OptionFactory.sol#L49](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L49)

```
        OptionPairLibrary.checkDoesNotExist(token0, token1, optionPair);
```
Suggested fix:

Replace the above code line with the following:

```
        if (optionPair != address(0)) Error.alreadyExisted(); // @ audit: Update this in Error.sol
```
Along with the sort logic implemented, [`OptionPair.sol`](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionPair.sol) could pretty much be done away.

## THE USE OF DELETE TO RESET VARIABLES
`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address or a boolean to false. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at x.

The delete key better conveys the intention and is also more idiomatic.

Here are the 3 specific examples.

[Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol)

```
228:            pool.long0ProtocolFees = 0;

236:            pool.long1ProtocolFees = 0;

244:            pool.shortProtocolFees = 0;
```
Suggested fix:

```
228:            delete pool.long0ProtocolFees;

236:            delete pool.long1ProtocolFees;

244:            delete pool.shortProtocolFees;
```
