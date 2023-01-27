## 1. `getByIndex`/`numberOfPairs()` is unusable

In `TimeswapV2OptionFactory.sol` and `TimeswapV2PoolFactory.sol`, The function `create()` never pushes any pairs into the `getByIndex` array. Because of this, `numberOfPairs()` will always return a length of zero.

Consider the removing both `getByIndex` and `numberOfPairs()` since they are unusable:

File: [TimeswapV2OptionFactory.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol)

```solidity
25:    address[] public override getByIndex;

36:    function numberOfPairs() external view override returns (uint256) {
37:        return getByIndex.length;
38:    }
```

File: [TimeswapV2PoolFactory.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol)

```solidity
33:    address[] public override getByIndex;

52:    function numberOfPairs() external view override returns (uint256) {
53:        return getByIndex.length;
54:    }
```

## 2. Using `storage` instead of `memory` for structs/arrays saves gas

When retrieving data from a `storage` location, assigning the data to a `memory` variable will cause all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declaring the variable with the memory keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incurring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another `memory` array/struct

File: `TimeswapV2LiquidityToken.sol` [Line 275](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L275)

```solidity
        FeesPosition memory feesPosition = _feesPositions[id][owner];
```

## 3. Redundant check

[Pool.sol#L160](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L160) will already check if `pool.liquidity != 0`. Because of this, calling `hasLiquidity()` is unnecessary as it is already checked.

Consider removing the `hasLiquidity()` check below:

File: `TimeswapV2Pool.sol` [Line 152-162](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L152-L162)

```solidity
152    function transferLiquidity(uint256 strike, uint256 maturity, address to, uint160 liquidityAmount) external override {
           /// @audit hasLiquidity()
153        hasLiquidity(strike, maturity);
154
155        if (blockTimestamp(0) > maturity) Error.alreadyMatured(maturity, blockTimestamp(0));
156        if (to == address(0)) Error.zeroAddress();
157        if (liquidityAmount == 0) Error.zeroInput();
158
159        pools[strike][maturity].transferLiquidity(to, liquidityAmount, blockTimestamp(0));
160
161        emit TransferLiquidity(strike, maturity, msg.sender, to, liquidityAmount);
162    }
```
