# Gas Optimizations Report for Timeswap contest
## Overview
During the audit, 3 gas issues were found.  

№ | Title | Instance Count 
--- | --- | --- 
G-1 | Use unchecked blocks for subtractions where underflow is impossible | 2 
G-2 | Using ```storage``` pointer to ```struct```/```array```/```mapping``` is cheaper than using ```memory``` | 3 
G-3 | Elements that are smaller than 32 bytes (256 bits) may increase gas usage | 15 

## Gas Optimizations Findings(3)
### G-1. Use unchecked blocks for subtractions where underflow is impossible
##### Description
In Solidity 0.8+, there’s a default overflow and underflow check on unsigned integers. When an overflow or underflow isn’t possible (after require or if-statement), some gas can be saved by using [unchecked blocks](https://docs.soliditylang.org/en/v0.8.17/control-structures.html#checked-or-unchecked-arithmetic).
##### Instances
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol#L15 (return BytesLib.slice(reason, 4, length - 4);)
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L319 (newRate = rate - deltaRate)

#
### G-2. Using ```storage``` pointer to ```struct```/```array```/```mapping``` is cheaper than using ```memory```
##### Description
When you copy a storage ```struct```/```array```/```mapping``` to a memory variable, you are copying each member by reading it from the storage, which is expensive, but when you use the ```storage``` keyword, you are just storing a pointer to the storage, which is much cheaper.
##### Instances
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L238
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L264
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L275

##### Recommendation
For example, change: 
```
FeesPosition memory feesPosition = _feesPositions[id][owner];
```  
to:  
```
FeesPosition storage feesPosition = _feesPositions[id][owner];
```

#
### G-3. Elements that are smaller than 32 bytes (256 bits) may increase gas usage
##### Description
According to [docs](https://docs.soliditylang.org/en/v0.8.16/internals/layout_in_storage.html#layout-of-state-variables-in-storage), when using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
##### Instances
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L16
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L34
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L54
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L72
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L16
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L42
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L43
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L44
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L12
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L15
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L18
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L55
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L70
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L72
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L90

##### Recommendation
Consider using a larger size where needed.