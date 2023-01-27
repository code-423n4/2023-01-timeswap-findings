### G-01 X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

*Number of Instances Identified: 52*

Using the addition operator instead of plus-equals saves **[113 gas]([https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)**

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol

```
141: option.totalLong0 -= token0AndLong0Amount;
142: option.totalLong1 -= token1AndLong1Amount;
173: option.totalLong0 -= token0AndLong0Amount
174: option.totalLong1 += token1AndLong1Amount;
177: option.totalLong1 -= token1AndLong1Amount;
178: option.totalLong0 += token0AndLong0Amount;
208: option.totalLong0 -= token0Amount;
209: option.totalLong1 -= token1Amount;
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol

```
149: longAmount -= fees;
150: shortAmount -= fees
180: shortAmount -= fees
212: longAmount -= fees
244: amount -= fees;
295:  denominator2 += longAmount;
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol

```
55: liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth);
56: liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth);
57: liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth);
66: liquidityPosition.liquidity += liquidityAmount;
70: liquidityPosition.long0Fees += long0Fees;
71: liquidityPosition.long1Fees += long1Fees;
72: liquidityPosition.shortFees += shortFees;
76: liquidityPosition.liquidity -= liquidityAmount;
80: liquidityPosition.long0Fees -= long0Fees;
81: liquidityPosition.long1Fees -= long1Fees;
82: liquidityPosition.shortFees -= shortFees;
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol

```
361: pool.long0Balance += long0Amount;
362: pool.long1Balance += long1Amount;
365: pool.liquidity += liquidityAmount;
452: pool.long0Balance -= long0Amount;
453: pool.long1Balance -= long1Amount;
455: pool.liquidity -= liquidityAmount;
537: pool.long0Balance += long0Amount;
538: pool.long1Balance += long1Amount;
636: pool.long0Balance -= (long0Amount + long0Fees);
650: pool.long1Balance -= (long1Amount + long1Fees);
677: pool.long1Balance -= (long1Amount + longFees);
689: pool.long1Balance -= (long1Amount + longFees);
695: pool.long0Balance += long0Amount;
710: pool.long0Balance -= (long0Amount + longFees);
719: pool.long0Balance -= (long0Amount + longFees);
722: pool.long1Balance += long1Amount;
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol

```
31: feesPosition.long0Fees += FeeCalculation.getFees(liquidity, feesPosition.long0FeeGrowth, long0FeeGrowth);
32: feesPosition.long1Fees += FeeCalculation.getFees(liquidity, feesPosition.long1FeeGrowth, long1FeeGrowth);
33: feesPosition.shortFees += FeeCalculation.getFees(liquidity, feesPosition.shortFeeGrowth, shortFeeGrowth);
53: feesPosition.long0Fees += long0Fees;
54: feesPosition.long1Fees += long1Fees;
55: feesPosition.shortFees += shortFees;
59: feesPosition.long0Fees -= long0Fees;
60: feesPosition.long1Fees -= long1Fees;
61: feesPosition.shortFees -= shortFees;
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol

```
95: _idTotalSupply[id] -= amount;
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol

```
163: productA1 += (quotient1 * divisor);
```

