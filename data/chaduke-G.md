G1. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2OptionFactory.sol#L49
Replacing it by a zero address check for ``optionPair`` will do it. That is what the function did anyway.
```
if (optionPair != address(0)) revert OptionPairAlreadyExisted(token0, token1, optionPair);
```

G2. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolDeployer.sol#L26-L28
It will be more gas-efficient to pass the Parameter struct as a calldata to the newly created ``TimeswapV2Pool``'s constructor. Passing by state variable is two expensive. 

G3. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L55
``listOfPools`` is not used and can be deleted along with its associated functions. 

G4. 
We can save gas here by checking whether the last fee growth and the global fee growth are equal. If yes, 1) there is no need to revise the last fee growth, which is a state variable, and 2) there is no need to calculate and add the fee either.  

```
function update(LiquidityPosition storage liquidityPosition, uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) internal {
        uint160 liquidity = liquidityPosition.liquidity;

        if (liquidity !=0){
            uint256 lastGrowth = liquidityPosition.long0FeeGrowth;
            if(lastLong0FeeGrowth != long0FeeGrowth){ 
                      liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, lastGrowth, long0FeeGrowth);
                      liquidityPosition.long0FeeGrowth = long0FeeGrowth;
            }

            lastGrowth = liquidityPosition.long1FeeGrowth;
            if(lastGrowth != long1FeeGrowth){ 
                     liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, lastGrowth, long1FeeGrowth);
                     liquidityPosition.long1FeeGrowth = long1FeeGrowth;
           }
  
           lastGrowth = liquidityPosition.shortFeeGrowth;
           if(lastGrowth != shortFeeGrowth){
                    liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, lastGrowth, shortFeeGrowth);
                    liquidityPosition.shortFeeGrowth = shortFeeGrowth;
           }
        }
    }
```