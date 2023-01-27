
### [Gas-01] ```<x> = <x> + <y>``` will cost less gas than ```<x> += <y>```
*Instances(36)*
```solidity
File:  v2-library/src/FullMath.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L163
```
```solidity
File:  v2-token/src/base/ERC1155Enumerable.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L61
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L95
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L117
```
```solidity
File:  v2-token/src/structs/FeesPosition.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L31-L33
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L53-L55
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L59-L63
```
```solidity
File:  v2-pool/src/libraries/ConstantProduct.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L149-L150
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L180
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L212
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L244
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L295
```
```solidity
File:  v2-pool/src/structs/LiquidityPosition.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L55-L57
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L66
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L70-L72
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L76
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L80-L82
```
```solidity
File:  v2-pool/src/structs/Pool.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L361-L362
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L365
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L452-L453
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L455
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L537-L538
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L722
```
```solidity
File:  v2-option/src/structs/Option.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L62-L63
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L65-L66
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L68-L69
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L103-L104
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L108-L109
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L112
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L141-L142
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L173-L175
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L177-L179
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L208-L209
```
```solidity
File:  v2-option/src/TimeswapV2Option.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L190-L192
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L237-L238
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L277
```



### [Gas-02] Arithmetic Operations that will not Overflow or Underflow should made ```uncheck```
*Instances(7)*

```solidity
File:  v2-library/src/FullMath.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L167
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L168
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L257
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L277
```
```solidity
File:  v2-library/src/Math.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L51
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L62
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L81
```


### [Gas-03] Structs can be more optimizable
Instade of a allocating 32bytes(uint256) to values like amount of ```long0Amount``` and ```long1Amount```, these could reduced to uint112 
so that both of them will store in a single slot and overall storage consumption will reduced as well they both easily get in a single call in corresponding functions.

*Instances(24)*
```solidity
File:  v2-token/src/structs/Param.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L26-L27
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L51-L52
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L110-L111
```
```solidity
File:  v2-token/src/structs/CallbackParam.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L18-L19
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L38-L39
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L87-L88
```
```solidity
File:  v2-pool/src/structs/CallbackParam.sol#
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L14-L15
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L31-L33
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L50-L51
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L69-L70
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L117-L118
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L134-L135
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L153-L154
```
```solidity
File:  v2-pool/src/structs/LiquidityPosition.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L17-L18
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L20-L21
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L55-L57
```
```solidity
File:  v2-pool/src/structs/Param.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L20-L21
```
```solidity
File:  v2-option/src/structs/CallbackParam.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/CallbackParam.sol#L14-L15
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/CallbackParam.sol#L30-L31
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/CallbackParam.sol#L49-L50
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/CallbackParam.sol#L64-L65
```
```solidity
File:  v2-option/src/structs/Param.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L23-L24
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L50-L51
```
```solidity
File:  v2-option/src/structs/Process.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L21-L22
```

### [Gas-04] Instead ```addr => addr => addr``` deep mapping try to use ```addr => struct```, means include all other inside a struct
*Instances(01)*

```solidity
File:  v2-option/src/TimeswapV2OptionFactory.sol
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L22
```


 