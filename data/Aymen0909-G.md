# Gas Optimizations

## Summary

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1 | Multiple address/IDs mappings can be combined into a single mapping of an address/id to a struct | 5 |
| 2 | `storage` variable should be cached into `memory` variables instead of re-reading them  |  1 |
| 3 | Use `unchecked` blocks to save gas  |  1 |
| 4 | `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables  | 58  |
| 5 | `memory` values should be emitted in events instead of `storage` ones  |  1 |

## Findings

### 1- Multiple address/IDs mappings can be combined into a single mapping of an address/id to a struct :

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

There are 5 instances of this issue :

File: v2-token/src/base/ERC1155Enumerable.sol [Line 15-17](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L15-L17)
```
mapping(address => mapping(uint256 => uint256)) private _ownedTokens; // An index of all tokens
mapping(address => uint256) private _currentIndex; // the current index for an address
```

These mappings could be refactored into the following struct and mapping for example :

```
struct Wallet {
    mapping(uint256 => uint256) _ownedTokens;
    uint256 _currentIndex;
}
    
mapping(address => Wallet) private _wellets;
```

File: v2-token/src/base/ERC1155Enumerable.sol [Line 20-28](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L20-L28)
```
20      mapping(uint256 => uint256) private _ownedTokensIndex;
22      mapping(uint256 => uint256) private _idTotalSupply;
28      mapping(uint256 => uint256) private _allTokensIndex;
```

These mappings could be refactored into the following struct and mapping for example :

```
struct Token {
    uint256 _ownedTokenIndex;
    uint256 _idTotalSupply;
    uint256 _allTokenIndex;
}
    
mapping(address => Token) private _tokens;
```

### 2- `storage` variable should be cached into `memory` variables instead of re-reading them :

The instances below point to the second+ access of a state variable within a function, the caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read, thus saves **100gas** for each instance.

There is 1 instances of this issue :

File: v2-option/src/structs/Option.sol [Line 192-206](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L192-L206)

```
uint256 denominator = StrikeConversion.combine(option.totalLong0, option.totalLong1, strike, true);

if (transaction == TimeswapV2OptionCollect.GivenShort) {
    shortAmount = amount;
    token0Amount = shortAmount.proportion(option.totalLong0, denominator, false);
    token1Amount = shortAmount.proportion(option.totalLong1, denominator, false);
} else if (transaction == TimeswapV2OptionCollect.GivenToken0) {
    token0Amount = amount;
    shortAmount = token0Amount.proportion(denominator, option.totalLong0, true);
    token1Amount = shortAmount.proportion(option.totalLong1, denominator, false);
} else if (transaction == TimeswapV2OptionCollect.GivenToken1) {
    token1Amount = amount;
    shortAmount = token1Amount.proportion(denominator, option.totalLong1, true);
    token0Amount = shortAmount.proportion(option.totalLong0, denominator, false);
}
```

In the code linked above the values of `option.totalLong0` and `option.totalLong1` are read multiple times (2) from storage and their respective values does not change so they should be cached into a memory variables in order to save gas by avoiding multiple reads from storage.


### 3- Use `unchecked` blocks to save gas :

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block.

There is 1 instance of this issue:

File: v2-pool/src/libraries/ConstantProduct.sol [Line 319](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L319)
```
else if (rate > deltaRate) newRate = rate - deltaRate;
```

The above operation cannot underflow due to the check that preceeds it so it should be marked as `unchecked`. 


### 4- `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables :

Using the addition operator instead of plus-equals saves **113 gas** as explained [here](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)

There are 58 instances of this issue:

```
File: v2-token/src/base/ERC1155Enumerable.sol

95      _idTotalSupply[id] -= amount;
117     _currentIndex[to] += 1;

File: v2-token/src/structs/FeesPosition.sol

31      feesPosition.long0Fees += FeeCalculation.getFees(liquidity, feesPosition.long0FeeGrowth, long0FeeGrowth);
32      feesPosition.long1Fees += FeeCalculation.getFees(liquidity, feesPosition.long1FeeGrowth, long1FeeGrowth);
33      feesPosition.shortFees += FeeCalculation.getFees(liquidity, feesPosition.shortFeeGrowth, shortFeeGrowth);
53      feesPosition.long0Fees += long0Fees;
54      feesPosition.long1Fees += long1Fees;
55      feesPosition.shortFees += shortFees;
59      feesPosition.long0Fees -= long0Fees;
60      feesPosition.long1Fees -= long1Fees;
61      feesPosition.shortFees -= shortFees;

File: v2-pool/src/structs/LiquidityPosition.sol

55      liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth);
56      liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth);
57      liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth);
66      liquidityPosition.liquidity += liquidityAmount;
70      liquidityPosition.long0Fees += long0Fees;
71      liquidityPosition.long1Fees += long1Fees;
72      liquidityPosition.shortFees += shortFees;
76      liquidityPosition.liquidity -= liquidityAmount;
80      liquidityPosition.long0Fees -= long0Fees;
81      liquidityPosition.long1Fees -= long1Fees;
82      liquidityPosition.shortFees -= shortFees;

File: v2-pool/src/structs/Pool.sol

361     pool.long0Balance += long0Amount;
362     pool.long1Balance += long1Amount;
365     pool.liquidity += liquidityAmount;
452     pool.long0Balance -= long0Amount;
453     pool.long1Balance -= long1Amount;
455     pool.liquidity -= liquidityAmount;
537     pool.long0Balance += long0Amount;
538     pool.long1Balance += long1Amount;
636     pool.long0Balance -= (long0Amount + long0Fees);
650     pool.long1Balance -= (long1Amount + long1Fees);
677     pool.long1Balance -= (long1Amount + longFees);
689     pool.long1Balance -= (long1Amount + longFees);
695     pool.long0Balance += long0Amount;
710     pool.long0Balance -= (long0Amount + longFees);
719     pool.long0Balance -= (long0Amount + longFees);
722     pool.long1Balance += long1Amount;

File: v2-option/src/structs/Option.sol

62      option.long0[msg.sender] -= amount;
63      option.long0[to] += amount;
65      option.long1[to] += amount;
66      option.long1[msg.sender] -= amount;
68      option.short[msg.sender] -= amount;
69      option.short[to] += amount;
103     option.totalLong0 += token0AndLong0Amount;
108     option.totalLong1 += token1AndLong1Amount;
109     option.long1[long1To] += token1AndLong1Amount;
112     option.short[shortTo] += shortAmount;
141     option.totalLong0 -= token0AndLong0Amount;
142     option.totalLong1 -= token1AndLong1Amount;
173     option.totalLong0 -= token0AndLong0Amount;
174     option.totalLong1 += token1AndLong1Amount;
175     option.long1[longTo] += token1AndLong1Amount;
178     option.totalLong1 -= token1AndLong1Amount;
179     option.totalLong0 += token0AndLong0Amount;
180     option.long0[longTo] += token0AndLong0Amount;
208     option.totalLong0 -= token0Amount;
209     option.totalLong1 -= token1Amount;
```

### 5- `memory` values should be emitted in events instead of `storage` ones :

Here, the values emitted shouldn’t be read from storage. The existing memory values should be used instead, this will save **~101 GAS**

There is 1 instance of this issue :

File: v2-pool/src/base/OwnableTwoSteps.sol [Line 31](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L31)
```
emit SetOwner(pendingOwner);
```

The storage `pendingOwner` variable is emitted which cost more gas, should instead emit memory variables `chosenPendingOwner` as follow :
```
emit SetOwner(chosenPendingOwner);
```
