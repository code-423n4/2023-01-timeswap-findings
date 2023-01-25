## [G-01] Sequence of tests.  

Comparison operations are match cheaper that loading. 
In case of wrong zero address or amount it will revert with less gas consumption. 

Consider moving hasLiquidity check lower then zero checks:
[TimeswapV2Pool.sol#152](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L152)
            
Consider moving hasInteracted check lower then zero checks:
[TimeswapV2Option.sol#97](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L97)
 
---


        
##  [G-02] Token0/token1 comparison 
While creating and getting the option pair there are two places with the checkCorrectFormat check that will revert transaction in case token0 < token1.

[TimeswapV2OptionFactory.sol#L30](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L30)
[TimeswapV2OptionFactory.sol#L46](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L46)


Consider using Uniswap v2 code in both instances to be sure that transaction wouldn't fail 

    (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
 
---

        
## [G-03] Two NoDelegateCall.sol contracts.
There are two same NoDelegateCall.sol contracts. One in pool folder, another in options. Consider moving it to v2-library and use same for both parts of the product and thus save gas on deployments.

