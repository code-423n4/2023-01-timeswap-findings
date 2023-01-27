### Gas Optimizations List
| Number | Optimization Details | Context |
|:--:|:-------| :-----:|
| [G-01] | Gas saving is achieved by removing the ``delete`` keyword (~60k) |1 |
| [G-02] |Remove ``checkDoesNotExist`` function|1 |
| [G-03] |Avoid using ``state variable`` in emit (130 gas) |1 |
| [G-04] |Change ``public`` state variable visibility to ``private``|2 |
| [G-05] |Save gas with the use of the import statement |1|
| [G-06] |Gas savings can be achieved by changing the model for assigning value to the structure (260 gas) |2|
| [G-07] |Using ``delete``  instead of setting struct ``0`` saves gas |10|
| [G-08] |In ``div 512`` function, ``quotient 0`` aggregate operation is used with unchecked to save gas |1|
| [G-09] |Avoid using external call |1|
| [G-10] |Gas overflow during iteration (DoS) |1|
| [G-11] |Move owner checks to a modifier for gas efficant |2|
| [G-12] |Use a more recent version of solidity |All contracts|
| [G-13] |Use nested if and, avoid multiple check combinations |19|
| [G-14] |] Sort Solidity operations using short-circuit mode|3|
| [G-15] |``>=`` costs less gas than ``>`` |4|
| [G-16] |Using UniswapV3 ``mulDiv`` function is gas-optimized |1|
| [G-17] |Using Openzeppelin Ownable2Step.sol is gas efficient |1|
| [G-18] |OpenZeppelin's ReentrancyGuard contract is gas-optimized ||
| [G-19] |Save gas with the use of the import statement||
| [G-20] |Remove import ``forge-std/console.sol`` |1|
| [G-21] |Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead |23|
| [G-22] |Use ``assembly`` to write _address storage values_  |2|
| [G-23] |Setting the _constructor_ to `payable` |8|
| [G-24] |Functions guaranteed to revert_ when callled by normal users can be marked `payable` (21 gas) |12|
| [G-25] |Avoid contract existence checks by using solidity version 0.8.10 or later |57|
| [G-26] |Optimize names to save gas |All contracts|
| [G-27] |Upgrade Solidity's optimizer|3|
| [G-28] |Open the optimizer |1|

Total 28 issues

### [G-01] Gas saving is achieved by removing the ``delete`` keyword (~60k)

30k gas savings were made by removing the ``delete`` keyword. The reason for using the ``delete`` keyword here is to reset the struct values (set to default value) in every operation. However, the struct values do not need to be zero each time the function is run. Therefore, the ``delete'' key word is unnecessary, if it is removed, around 30k gas savings will be achieved.


There are two instances of the subject:

```diff
packages\v2-option\src\TimeswapV2OptionDeployer.sol:
  31      /// @return optionPair The address of the newly deployed TimeswapV2Option contract.
  32:     function deploy(address optionFactory, address token0, address token1) internal returns (address optionPair) {
  33:         parameter = Parameter({optionFactory: optionFactory, token0: token0, token1: token1});
  34: 
  35:         optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
  36: 
  37:         // save gas.
- 38:         delete parameter;
  39:     }
  40  }
```

```diff
packages\v2-pool\src\TimeswapV2PoolDeployer.sol:
  24  
  25:     function deploy(address poolFactory, address optionPair, uint256 transactionFee, uint256 protocolFee) internal returns (address poolPair) {
  26:         parameter = Parameter({poolFactory: poolFactory, optionPair: optionPair, transactionFee: transactionFee, protocolFee: protocolFee});
  27: 
  28:         poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
  29: 
- 30:         delete parameter;
  31:     }
  32  }
```

### [G-02] Remove ``checkDoesNotExist`` function

Using separate internal functions for non-repeating if blocks in more than one function wastes gas.

The `checkDoesNotExist` _internal_ function in the `OptionPair.sol` contract is only used in the ``create`` function of the `TimeswapV2OptionFactory.sol` contract.

```solidity 
packages\v2-option\src\TimeswapV2OptionFactory.sol:
  43:     function create(address token0, address token1) external override returns (address optionPair) {
  44:         if (token0 == address(0)) Error.zeroAddress();
  45:         if (token1 == address(0)) Error.zeroAddress();
  46:         OptionPairLibrary.checkCorrectFormat(token0, token1);
  47: 
  48:         optionPair = optionPairs[token0][token1];
  49:         OptionPairLibrary.checkDoesNotExist(token0, token1, optionPair);
  50: 
  51:         optionPair = deploy(address(this), token0, token1);
  52: 
  53:         optionPairs[token0][token1] = optionPair;
  54: 
  55:         emit Create(msg.sender, token0, token1, optionPair);
  56:     }
  57  }
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L43-L57

**Recommendation:**
I suggest that the check on line 50 be done by adding the **if** block as follows.

**_packages\v2-option\src\TimeswapV2OptionFactory.sol_**
```diff
  43:     function create(address token0, address token1) external override returns (address optionPair) {
  44:         if (token0 == address(0)) Error.zeroAddress();
  45:         if (token1 == address(0)) Error.zeroAddress();
  46:         OptionPairLibrary.checkCorrectFormat(token0, token1);
  47: 
  48:         optionPair = optionPairs[token0][token1];
- 49:         OptionPairLibrary.checkDoesNotExist(token0, token1, optionPair);
+                   if (optionPair != address(0)) revert OptionPairAlreadyExisted(token0, token1, optionPair);
  50: 
  51:         optionPair = deploy(address(this), token0, token1);
  52: 
  53:         optionPairs[token0][token1] = optionPair;
  54: 
  55:         emit Create(msg.sender, token0, token1, optionPair);
  56:     }
  57  }
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L43-L57


### [G-03] Avoid using ``state variable`` in emit (130 gas)

Using a state variable in ``SetOwner`` emits wastes gas.

1 result - 1 file:
```solidity
packages\v2-pool\src\base\OwnableTwoSteps.sol:
  23     function setPendingOwner(address chosenPendingOwner) external override {
  24         Ownership.checkIfOwner(owner);
  25 
  26:        if (chosenPendingOwner == address(0)) Error.zeroAddress();
  27         chosenPendingOwner.checkIfAlreadyOwner(owner);
  28 
  29         pendingOwner = chosenPendingOwner;
  30 
  31:         emit SetOwner(pendingOwner);
  32:     }
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L31

If the following recommendation is taken into account, 130 gas is saved.

```diff
packages\v2-pool\src\base\OwnableTwoSteps.sol:
 23     function setPendingOwner(address chosenPendingOwner) external override {
  24         Ownership.checkIfOwner(owner);
  25 
  26         if (chosenPendingOwner == address(0)) Error.zeroAddress();
  27         chosenPendingOwner.checkIfAlreadyOwner(owner);
  28 
+ 31:         emit SetOwner(chosenPendingOwner);
  29         pendingOwner = chosenPendingOwner;
  30 
- 31:         emit SetOwner(pendingOwner);
  32     }
```

### [G-04] Change ``public`` state variable visibility to ``private``

If it is preferred to change the visibility of the `owner` and `pendingOwnerstate` state variables to ``private``, this will save significant gas.

2 result - 1 file:
```solidity
packages\v2-pool\src\base\OwnableTwoSteps.sol:
  14:     address public override owner;

  16:     address public override pendingOwner;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L14-L16


### [G-05] Save gas with the use of the import statement

While the following two critical fee values are assigned in the constructor, there is no zero value control. This means that if both state variables are started with a possible value of ``0``, the contract must be deployed again. This possibility means gas consumption.

Zero value control is the most error-prone value control since zero value is assigned in case of no value entry due to EVM design.

In addition, since the immutable value will be changed once, adding a zero value control does not cause high gas consumption.

```solidity
packages\v2-pool\src\TimeswapV2PoolFactory.sol:
  37:     constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) {
  38:         if (chosenTransactionFee > type(uint16).max) revert IncorrectFeeInitialization(chosenTransactionFee);
  39:         if (chosenProtocolFee > type(uint16).max) revert IncorrectFeeInitialization(chosenProtocolFee);
  40: 
  41:         transactionFee = chosenTransactionFee;
  42:         protocolFee = chosenProtocolFee;
  43:     }
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37-L43


**Recommendation step:**
It is recommended to perform a zero value check for critical value assignments.

Add zero check for immutable values when assigning values in critical constructor.


### [G-06] Gas savings can be achieved by changing the model for assigning value to the structure (260 gas)

By changing the pattern of assigning value to the structure, gas savings of ``~130`` per instance are achieved. In addition, this use will provide significant savings in distribution costs.

There are two examples of this issue:

```solidity
packages\v2-pool\src\TimeswapV2PoolDeployer.sol:
  25:     function distribution(pool Factory address, option pair address, uint256 transaction Fee, uint256 protocol Fee) internal returns (poolPair address) {
  26:         parameter = Parameter({poolFactory: poolFactory, optionPair: optionPair, transactionFees: transactionFees, protocolFees: protocolFees});
```


```solidity
packages\v2-option\src\TimeswapV2OptionDeployer.sol:
  32:     function deploy(address optionFactory, address identifier0, address identifier1) internal returns (address optionPair) {
  33:         parameter = Parameter({optionFactory:optionFactory, symbol0: symbol0, symbol1: symbol1});
```

The following model, which is more gas efficient, can be preferred to assign value to the building elements.

```diff
packages\v2-option\src\TimeswapV2OptionDeployer.sol:
  32:     function deploy(address optionFactory, address identifier0, address identifier1) internal returns (address optionPair) {
- 33:         parameter = Parameter({optionFactory: optionsFactory, token0: token0, token1: token1});
+              parameter.optionFactory = optionFactory;
+              parameter.token0 = token0;
+              parameter.token1 = token1;
```


### [G-07] Using ``delete``  instead of setting struct ``0`` saves gas

10 results - 2 files:
```solidity
packages\v2-pool\src\structs\LiquidityPosition.sol:
   93:    liquidityPosition.long0Fees = 0;

  101:    liquidityPosition.long1Fees = 0;

  109:    liquidityPosition.shortFees = 0;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L93


```solidity
packages\v2-pool\src\structs\Pool.sol:
  228:    pool.long0ProtocolFees = 0;

  236:    pool.long1ProtocolFees = 0;

  244:    pool.shortProtocolFees = 0;

  633:    pool.long0Balance = 0;

  646:    pool.long1Balance = 0;

  685:    pool.long1Balance = 0;

  706:    pool.long0Balance = 0;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L228


**Recommendation code:**
```diff
packages\v2-pool\src\structs\Pool.sol#L228
-  228:    pool.long0ProtocolFees = 0;
+ 228:    delete pool.long0ProtocolFees;
```

### [G-08] In ``div 512`` function, ``quotient 0`` aggregate operation is used with unchecked to save gas

```diff
packages/v2-library/src/FullMath.sol:
  158:     function div512(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient0, uint256 quotient1) {
  159:         (quotient0, quotient1) = div512(dividend0, dividend1, divisor);
  160: 
  161:         if (roundUp) {
  162:             (uint256 productA0, uint256 productA1) = mul512(quotient0, divisor);
  163:             productA1 += (quotient1 * divisor);
  164:             if (dividend1 > productA1 || dividend0 > productA0) {
  165:                 if (quotient0 == type(uint256).max) {
  166:                     quotient0 = 0;
  167:                     quotient1++;
- 168:                 } else quotient0++;
+ 168:                 } else 
+                         unchecked {
+                          quotient0++;
+                         } 
  169:             }
  170:         }
  171:     }
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L165-L168

### [G-09] Avoid using external call

An if block check can be added as follows. With this control, gas saving is achieved by avoiding the use of external calls.

```diff
packages/v2-pool/src/base/OwnableTwoSteps.sol:
  22      /// @inheritdoc IOwnableTwoSteps
  23:     function setPendingOwner(address chosenPendingOwner) external override {
- 24:         Ownership.checkIfOwner(owner);
+ 24:         if (msg.sender == owner) revert NotOwner();
  25: 
  26:         if (chosenPendingOwner == address(0)) Error.zeroAddress();
  27:         chosenPendingOwner.checkIfAlreadyOwner(owner);
  28: 
  29:         pendingOwner = chosenPendingOwner;
  30: 
  31:         emit SetOwner(pendingOwner);
  32:     }
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L24


### [G-10] Gas overflow during iteration (DoS)

Each iteration of the cycle requires a gas flow. A moment may come when more gas is required than it is allocated to record one block. In this case, all iterations of the loop will fail.

```diff
packages/v2-option/src/structs/Process.sol:
  32      /// @param isAddToken1 IsAddToken1 if true. IsSubToken0 if false
  33:     function updateProcess(Process[] storage processing, uint256 token0Amount, uint256 token1Amount, bool isAddToken0, bool isAddToken1) internal {
+              require(processing.length.length() < maxProcessingLengt, "max length");
  34:         for (uint256 i; i < processing.length; ) {
  35:             Process storage process = processing[i];
  36: 
  37:             if (token0Amount != 0) process.balance0Target = isAddToken0 ? process.balance0Target + token0Amount : process.balance0Target - token0Amount;
  38: 
  39:             if (token1Amount != 0) process.balance1Target = isAddToken1 ? process.balance1Target + token1Amount : process.balance1Target - token1Amount;
  40: 
  41:             unchecked {
  42:                 i++;
  43:             }
  44:         }
  45:      }
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L33-L45

### [G-11] Move owner checks to a modifier for gas efficant

It's better to use a modifier for simple owner checks for an easier inspection of functions. This is also more gas efficient as it does not control with external call.

**The part where ``owner`` is defined:**
```solidity
packages/v2-library/src/Ownership.sol:
  22:     function checkIfOwner(address owner) internal view {
  23          if (msg.sender != owner) revert NotTheOwner(msg.sender, owner);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Ownership.sol#L22-L23


2 results 2 files:
```solidity
packages/v2-pool/src/TimeswapV2Pool.sol:
  189:         ITimeswapV2PoolFactory(poolFactory).owner().checkIfOwner();
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L189

```solidity
packages/v2-pool/src/base/OwnableTwoSteps.sol:
  23      function setPendingOwner(address chosenPendingOwner) external override {
  24:         Ownership.checkIfOwner(owner);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L23-L24


### [G-12] Use a more recent version of solidity

> Solidity 0.8.10 has a useful change that reduced gas costs of external calls which expect a return value. 
> 
> In 0.8.15 the conditions necessary for inlining are relaxed. Benchmarks show that the change significantly decreases the bytecode size (which impacts the deployment cost) while the effect on the runtime gas usage is smaller.
> 
> In 0.8.17 prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call; Simplify the starting offset of zero-length operations to zero. More efficient overflow checks for multiplication.

The version of 70 contracts included in the scope is ``0.8.8``. I recommend that you upgrade the versions of all contracts in scope to the latest version of robustness, '0.8.17’.


### [G-13] Use nested if and, avoid multiple check combinations

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

19 results - 9 files:
```solidity
packages\v2-library\src\CatchError.sol:
  15:    if ((length - 4) % 32 == 0 && bytes4(reason) == selector) return BytesLib.slice(reason, 4, length - 4);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol#L15

```solidity
packages\v2-library\src\FullMath.sol:
  257:   if (roundUp && mulmod(multiplicand, multiplier, divisor) != 0) result++;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L257

```solidity
packages\v2-library\src\Math.sol:
  51:    if (roundUp && dividend % divisor != 0) quotient++;

  62:    if (roundUp && dividend % (1 << divisorBit) != 0) quotient++;
 
  81:    if (roundUp && value % result != 0) result++;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L51

```solidity
packages\v2-option\src\structs\Param.sol:
  111:   if (param.amount0 == 0 && param.amount1 == 0) Error.zeroInput();

  124:   if (param.amount0 == 0 && param.amount1 == 0) Error.zeroInput();
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L111

```solidity
packages\v2-pool\src\TimeswapV2Pool.sol:
  167:   if (long0Fees == 0 && long1Fees == 0 && shortFees == 0) Error.zeroInput();
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L167

```solidity
packages\v2-pool\src\libraries\ConstantProduct.sol:
  411:   if (a11 == 0 && a01.unsafeAdd(a10) >= a01) {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L411

```solidity
packages\v2-pool\src\structs\Param.sol:
  136:   if (param.long0Requested == 0 && param.long1Requested == 0 && param.shortRequested == 0 && param.strike == 0) Error.zeroInput();
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L136

```solidity
packages\v2-token\src\base\ERC1155Enumerable.sol:
  60:    if (_idTotalSupply[id] == 0 && _additionalConditionAddTokenToAllTokensEnumeration(id)) _addTokenToAllTokensEnumeration(id);

  64:    if (to != address(0) && to != from) {
  
  65:    if (balanceOf(to, id) == 0 && _additionalConditionAddTokenToOwnerEnumeration(to, id)) _addTokenToOwnerEnumeration(to, id);

  94:    if (_idTotalSupply[id] == 0 && _additionalConditionRemoveTokenFromAllTokensEnumeration(id)) _removeTokenFromAllTokensEnumeration(id);

  98:    if (from != address(0) && from != to) {
  
  99:    if (balanceOf(from, id) == 0 && _additionalConditionRemoveTokenFromOwnerEnumeration(from, id)) _removeTokenFromOwnerEnumeration(from, id);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L60

```solidity
packages\v2-token\src\structs\Param.sol:
  121:   if (param.long0Amount == 0 && param.long1Amount == 0 && param.shortAmount == 0) Error.zeroInput();

  128:   if (param.long0Amount == 0 && param.long1Amount == 0 && param.shortAmount == 0) Error.zeroInput();

  149:   if (param.long0FeesDesired == 0 && param.long1FeesDesired == 0 && param.shortFeesDesired == 0) Error.zeroInput();
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L121

**Recomendation Code:**
```diff
protocol\contracts\plugins\aave\StaticATokenLM.sol#L422:
- 149:   if (param.long0FeesDesired == 0 && param.long1FeesDesired == 0 && param.shortFeesDesired == 0) Error.zeroInput();
+           if (param.long0FeesDesired == 0) {
+               if (param.long1FeesDesired == 0) {
+                   if (param.shortFeesDesired == 0) {
+                       Error.zeroInput();
+	 }
+               }                                   
+           }                                     
```

### [G-14] Sort Solidity operations using short-circuit mode

Short-circuiting is a solidity contract development model that uses ```OR/AND``` logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.

```
//f(x) is a low gas cost operation 
//g(y) is a high gas cost operation 

//Sort operations with different gas costs as follows 
f(x) || g(y) 
f(x) && g(y)
```
3 results - 3 files:
```solidity
packages\v2-pool\src\libraries\ConstantProduct.sol:
  298:    if (product.div(longAmount, false) != rate || product >= numerator) revert NotEnoughLiquidityToBorrow();
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L298

```solidity
packages\v2-library\src\CatchError.sol:
  15:     if ((length - 4) % 32 == 0 && bytes4(reason) == selector) return BytesLib.slice(reason, 4, length - 4);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol#L15

```solidity
packages\v2-library\src\FullMath.sol:
  68:     if (subtrahend1 > minuend1 || (subtrahend1 == minuend1 && subtrahend0 > minuend0)) revert SubUnderflow(minuend0, minuend1, subtrahend0, subtrahend1);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L68

### [G-15] ``>=`` costs less gas than ``>``

The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas

4 results - 2 files:
```solidity
packages\v2-library\src\Math.sol:
  89:         return value1 < value2 ? value1 : value2;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L89

```solidity
packages\v2-library\src\StrikeConversion.sol:
  27:         return strike > type(uint128).max ? (toOne ? convert(amount, strike, true, roundUp) : amount) : (toOne ? amount : convert(amount, strike, false, roundUp));

  36:         return strike > type(uint128).max ? amount0 + convert(amount1, strike, false, roundUp) : amount1 + convert(amount0, strike, true, roundUp);

  48:             strike > type(uint128).max
  49                 ? (zeroToOne ? convert(base - amount, strike, true, roundUp) : base - convert(amount, strike, false, !roundUp))
  50                 : (zeroToOne ? base - convert(amount, strike, true, !roundUp) : convert(base - amount, strike, false, roundUp));
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol#L27

### [G-16] Using UniswapV3 ``mulDiv`` function is gas-optimized

```solidity
packages/v2-library/src/FullMath.sol:
  180      /// @return result The result.
  181:     function mulDiv(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) internal pure returns (uint256 result) {
  182         (uint256 product0, uint256 product1) = mul512(multiplicand, multiplier);
  183 
  184         // Handle non-overflow cases, 256 by 256 division
```


Reference: https://github.com/Uniswap/v3-core/blob/412d9b236a1e75a98568d49b1aeb21e3a1430544/contracts/libraries/FullMath.sol#L14

Reference: ttps://xn--2-umb.com/21/muldiv/

### [G-17] Using Openzeppelin Ownable2Step.sol is gas efficient

The project makes secure Owner changes with OwnableTwoStep.

The project's acceptOwner() function;

```solidity
packages\v2-pool\src\base\OwnableTwoSteps.sol:
  34      /// @inheritdoc IOwnableTwoSteps
  35:     function acceptOwner() external override {
  36:         msg.sender.checkIfPendingOwner(pendingOwner);
  37: 
  38:         owner = msg.sender;
  39:         delete pendingOwner;
  40: 
  41:         emit AcceptOwner(msg.sender);
  42:     }
  43  }
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L35-L43


However, I recommend using the more gas-optimized Openzeppelin in Ownable2Step.sol.

Openzeppelin acceptOwner() function;
```solidity
function acceptOwnership() public virtual {
        address sender = _msgSender();
        require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
        _transferOwnership(sender);
    }
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol


### [G-18] OpenZeppelin's ReentrancyGuard contract is gas-optimized

```solidity
packages/v2-pool/src/libraries/ReentrancyGuard.sol:
  11:     /// @dev The initial state which must be change to NOT_ENTERED when first interacting.
  12:     uint96 internal constant NOT_INTERACTED = 0;
  13: 
  14:     /// @dev The initial and ending state of balanceTarget in the Option struct.
  15:     uint96 internal constant NOT_ENTERED = 1;
  16: 
  17:     /// @dev The state where the contract is currently being interacted with.
  18:     uint96 internal constant ENTERED = 2;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L12-L18


I recommend using the gas-optimized OpenZeppelin ReentrancyGuard.sol contract.

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol


### [G-19] Save gas with the use of the import statement

With the import statement, it saves gas to specifically import only the parts of the contracts, not the complete ones.

```solidity
packages/v2-token/src/interfaces/IERC1155Enumerable.sol:

6: import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
```

**Description:**
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct `polluted the source code` with an unnecessary object we were not using because we did not need it. 
This was breaking the rule of modularity and modular programming: `only import what you need` Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
`import {contract1 , contract2} from "filename.sol";`

A good example from the ArtGobblers project;

import {Owned} from "solmate/auth/Owned.sol";
import {ERC721} from "solmate/tokens/ERC721.sol";
import {LibString} from "solmate/utils/LibString.sol";
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";


### [G-20] Remove import ``forge-std/console.sol``

It's used to print the values of variables while running tests to help debug and see what's happening inside your contracts But since it's a development tool, it serves no purpose on mainnet. 

1 result - 1 file:
```solidity
packages\v2-token\src\TimeswapV2Token.sol:
  5: import "forge-std/console.sol"; 
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L5


Also, the following code block should be removed along with the removal of ``forge-std/console.sol``.

```solidity
packages/v2-token/src/TimeswapV2Token.sol:
  109:             console.log("reaches right before mint in timeswapv2Tokne::mint");
```

**Recommendation:**
Use only for tests

### [G-21] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contracts gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html 

Use a larger size then downcast where needed.

23 results - 4 files:
```solidity
packages\v2-library\src\SafeCast.sol:
  20:    result = uint16(value);

  38:    result = uint160(value);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/SafeCast.sol#L20

```solidity
packages\v2-pool\src\TimeswapV2Pool.sol:
  104:   return pools[strike][maturity].liquidity;

  109:   return pools[strike][maturity].sqrtInterestRate;

  114:   return pools[strike][maturity].liquidityPositions[owner].liquidity;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L104

```solidity
packages\v2-pool\src\libraries\ConstantProduct.sol:
  67:    liquidityAmount = getLiquidityGivenLong(rate, longAmount, !isAdd);

  82:    liquidityAmount = getLiquidityGivenShort(rate, shortAmount, duration, !isAdd);

  104:   liquidityAmount = getLiquidityGivenLong(rate, amount, !isAdd);

  110:   liquidityAmount = getLiquidityGivenShort(rate, amount, duration, !isAdd);

  142:   newRate = isAdd ? rate + deltaRate : rate - deltaRate;

  174:   newRate = getNewSqrtInterestRateGivenLong(liquidity, rate, longAmount + (isAdd ? 0 : fees), isAdd);

  206:   (newRate, deltaRate) = getNewSqrtInterestRateGivenShort(liquidity, rate, shortAmount + (isAdd ? 0 : fees), duration, isAdd);

  260:   return FullMath.mulDiv(uint256(rate), longAmount, uint256(1) << 96, roundUp).toUint160();

  269:   return FullMath.mulDiv(shortAmount, uint256(1) << 192, uint256(rate).unsafeMul(duration), roundUp).toUint160();

  296:   return numerator.div(denominator2, true).toUint160();

  316:   deltaRate = FullMath.mulDiv(shortAmount, uint256(1) << 192, denominator, !isAdd).toUint160();
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L67

```solidity
packages\v2-pool\src\structs\LiquidityPosition.sol:
  66:    liquidityPosition.liquidity += liquidityAmount;

  76:    liquidityPosition.liquidity -= liquidityAmount;

  207:   pool.sqrtInterestRate = rate;

  308:   liquidityAmount = param.delta.toUint160(),

  398:   liquidityAmount = param.delta.toUint160(),

  486:   param.delta.toUint160(),

  572:   param.delta.toUint160(),
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L66


### [G-22] Use ``assembly`` to write _address storage values_ 

2 results - 2 files:
```solidity
packages\v2-pool\src\base\OwnableTwoSteps.sol:
  18     constructor(address chosenOwner) {
  19:         owner = chosenOwner;```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L19


```solidity
packages\v2-token\src\TimeswapV2Token.sol:
  41     constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {
  42:           optionFactory = chosenOptionFactory;
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L42


**Recommendation Code:**
```diff
  41     constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {
- 42:           optionFactory = chosenOptionFactory;
+                  assembly {                      
+                      sstore(optionFactory.slot, chosenOptionFactory)
+                  }                               
```

### [G-23] Setting the _constructor_ to `payable`

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ```msg.value == 0``` and saves ```13 gas``` on deployment with no security risks.

8 results - 8 files:
```solidty
packages\v2-option\src\NoDelegateCall.sol:
  19:     constructor() {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/NoDelegateCall.sol#L19


```solidty
packages\v2-option\src\TimeswapV2Option.sol:
  65:     constructor() NoDelegateCall() {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L65


```solidty
packages\v2-pool\src\NoDelegateCall.sol:
  19:     constructor() {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/NoDelegateCall.sol#L19


```solidty
packages\v2-pool\src\TimeswapV2Pool.sol:
  77:     constructor() NoDelegateCall() {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L77


```solidty
packages\v2-pool\src\TimeswapV2PoolFactory.sol:
  37:     constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37


```solidty
packages\v2-pool\src\base\OwnableTwoSteps.sol:
  18:     constructor(address chosenOwner) {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L18


```solidty
packages\v2-token\src\TimeswapV2LiquidityToken.sol:
  36:     constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36

```solidty
packages\v2-token\src\TimeswapV2Token.sol:
  41:     constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L41



**Recommendation:**
Set the constructor to ```payable```


### [G-23] Setting the _constructor_ to `payable`

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ```msg.value == 0``` and saves ```13 gas``` on deployment with no security risks.

8 results - 8 files:
```solidty
packages\v2-option\src\NoDelegateCall.sol:
  19:     constructor() {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/NoDelegateCall.sol#L19


```solidty
packages\v2-option\src\TimeswapV2Option.sol:
  65:     constructor() NoDelegateCall() {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L65


```solidty
packages\v2-pool\src\NoDelegateCall.sol:
  19:     constructor() {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/NoDelegateCall.sol#L19


```solidty
packages\v2-pool\src\TimeswapV2Pool.sol:
  77:     constructor() NoDelegateCall() {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L77


```solidty
packages\v2-pool\src\TimeswapV2PoolFactory.sol:
  37:     constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37


```solidty
packages\v2-pool\src\base\OwnableTwoSteps.sol:
  18:     constructor(address chosenOwner) {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L18


```solidty
packages\v2-token\src\TimeswapV2LiquidityToken.sol:
  36:     constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36

```solidty
packages\v2-token\src\TimeswapV2Token.sol:
  41:     constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L41



**Recommendation:**
Set the constructor to ```payable```


### [G-25] Avoid contract existence checks by using solidity version 0.8.10 or later

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

57 results - 6 files:
```solidity
packages\v2-token\src\TimeswapV2LiquidityToken.sol:
   66:    amount = balanceOf(owner, _timeswapV2LiquidityTokenPositionIds[timeswapV2LiquidityTokenPosition.toKey()]);

   71:    safeTransferFrom(from, to, _timeswapV2LiquidityTokenPositionIds[timeswapV2LiquidityTokenPosition.toKey()], liquidityAmount, bytes(""));

   81:    uint256 id = _timeswapV2LiquidityTokenPositionIds[position.toKey()];
 
  109:    bytes32 key = timeswapV2LiquidityTokenPosition.toKey();
  
  153:    bytes32 key = TimeswapV2LiquidityTokenPosition({token0: param.token0, token1: param.token1, strike: param.strike, maturity: param.maturity}).toKey();
 
  185:    bytes32 key = TimeswapV2LiquidityTokenPosition({token0: param.token0, token1: param.token1, strike: param.strike, maturity: param.maturity}).toKey();

  125:    uint160 liquidityBalanceTarget = ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)) + param.liquidityAmount;

  143:    Error.checkEnough(ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)), liquidityBalanceTarget);

  125:    uint160 liquidityBalanceTarget = ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)) + param.liquidityAmount;

  143:    Error.checkEnough(ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)), liquidityBalanceTarget);

  131:    data = ITimeswapV2LiquidityTokenMintCallback(msg.sender).timeswapV2LiquidityTokenMintCallback(

  163:    data = ITimeswapV2LiquidityTokenBurnCallback(msg.sender).timeswapV2LiquidityTokenBurnCallback(

  160:    ITimeswapV2Pool(poolPair).transferLiquidity(param.strike, param.maturity, param.to, param.liquidityAmount);

  193:    ITimeswapV2Pool(poolPair).transferFees(param.strike, param.maturity, param.to, long0Fees, long1Fees, shortFees);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L66

```solidity
packages\v2-token\src\TimeswapV2Token.sol:
   67:    amount = ERC1155.balanceOf(owner, _timeswapV2TokenPositionIds[timeswapV2TokenPosition.toKey()]);

   72:    safeTransferFrom(from, to, _timeswapV2TokenPositionIds[timeswapV2TokenPosition.toKey()], (amount), bytes(""));

   97:    bytes32 key = timeswapV2TokenPosition.toKey();
  
  127:    bytes32 key = timeswapV2TokenPosition.toKey();
 
  156:    bytes32 key = timeswapV2TokenPosition.toKey();
  
  240:    _burn(msg.sender, _timeswapV2TokenPositionIds[timeswapV2TokenPosition.toKey()], param.long0Amount);

  254:    _burn(msg.sender, _timeswapV2TokenPositionIds[timeswapV2TokenPosition.toKey()], param.long1Amount);

  268:    _burn(msg.sender, _timeswapV2TokenPositionIds[timeswapV2TokenPosition.toKey()], param.shortAmount);

   87:    long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + param.long0Amount;

  117:    long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + param.long1Amount;

  146:    shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + param.shortAmount;

  186:    if (param.long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);

  189:    if (param.long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);

  192:    if (param.shortAmount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L67

```solidity
packages\v2-pool\src\TimeswapV2Pool.sol:
  189:    ITimeswapV2PoolFactory(poolFactory).owner().checkIfOwner();

  267:    if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;
  
  271:    if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;
  
  274:    uint256 shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;
  
  293:    if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);
  
  295:    if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);
  
  297:    Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);
  
  393:    if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;
  
  397:    if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;
  
  411:    if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);
  
  413:    if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);
  
  444:    uint256 balanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;
  
  461:    Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), balanceTarget);
  
  479:    uint256 balanceTarget = ITimeswapV2Option(optionPair).positionOf(
  
  511:    ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), param.isLong0ToLong1 ? TimeswapV2OptionPosition.Long0 : TimeswapV2OptionPosition.Long1),
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L189

```solidity
packages\v2-option\src\libraries\OptionFactory.sol:
  28:     optionPair = ITimeswapV2OptionFactory(optionFactory).get(token0, token1);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionFactory.sol#L28

```solidity
packages\v2-pool\src\libraries\PoolFactory.sol:
  32:     poolPair = ITimeswapV2PoolFactory(poolFactory).get(optionPair);
  
  46:     poolPair = ITimeswapV2PoolFactory(poolFactory).get(optionPair);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L32

```solidity
packages\v2-option\src\TimeswapV2Option.sol:
  128:    IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
  
  129:    IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount
  
  145:    if (token0AndLong0Amount != 0) Error.checkEnough(IERC20(token0).balanceOf(address(this)), currentProcess.balance0Target);
  
  148:    if (token1AndLong1Amount != 0) Error.checkEnough(IERC20(token1).balanceOf(address(this)), currentProcess.balance1Target);
  
  215:    param.isLong0ToLong1 ? IERC20(token0).balanceOf(address(this)) - token0AndLong0Amount : IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
  
  216:    param.isLong0ToLong1 ? IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount : IERC20(token1).balanceOf(address(this)) - token1AndLong1Amount
  
  172:    if (token0AndLong0Amount != 0) IERC20(token0).safeTransfer(param.token0To, token0AndLong0Amount);
  
  175:    if (token1AndLong1Amount != 0) IERC20(token1).safeTransfer(param.token1To, token1AndLong1Amount);
  
  220:    IERC20(param.isLong0ToLong1 ? token0 : token1).safeTransfer(param.tokenTo, param.isLong0ToLong1 ? token0AndLong0Amount : token1AndLong1Amount);
  
  259:    if (token0Amount != 0) IERC20(token0).safeTransfer(param.token0To, token0Amount);
  
  262:    if (token1Amount != 0) IERC20(token1).safeTransfer(param.token1To, token1Amount);
```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L128

### [G-26] Optimize names to save gas

Contracts most called functions could simply save gas by function ordering via ```Method ID```. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because ```22 gas``` are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

**Context:** 
All Contracts


**Recommendation:** 
Find a lower ```method ID``` name for the most called functions for example Call() vs. Call1() is cheaper by ```22 gas```
For example, the function IDs in the ```TimeswapV2Pool.sol``` contract will be the most used; A lower method ID may be given.

**Proof of Consept:**
https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

TimeswapV2Pool.sol function names can be named and sorted according to METHOD ID

```js
Sighash   |   Function Signature
========================
fbddf051  =>  addPoolEnumerationIfNecessary(uint256,uint256)
1ea3a4eb  =>  raiseGuard(uint256,uint256)
3a9e8dd9  =>  lowerGuard(uint256,uint256)
f4f89897  =>  blockTimestamp(uint96)
2d883a73  =>  getByIndex(uint256)
6f682a53  =>  numberOfPools()
5c81c9b8  =>  hasLiquidity(uint256,uint256)
b15044ac  =>  totalLiquidity(uint256,uint256)
a8f403b7  =>  sqrtInterestRate(uint256,uint256)
f78333a3  =>  liquidityOf(uint256,uint256,address)
647284f5  =>  feeGrowth(uint256,uint256)
6c867790  =>  feesEarnedOf(uint256,uint256,address)
72f8f85c  =>  protocolFeesEarned(uint256,uint256)
7ffd3a70  =>  totalLongBalance(uint256,uint256)
3a9d71e7  =>  totalLongBalanceAdjustFees(uint256,uint256)
8fdc5c99  =>  totalPositions(uint256,uint256)
c0e0c4c6  =>  transferLiquidity(uint256,uint256,address,uint160)
d7e2c24a  =>  transferFees(uint256,uint256,address,uint256,uint256,uint256)
ad118b02  =>  initialize(uint256,uint256,uint160)
47b46959  =>  collectProtocolFees(TimeswapV2PoolCollectParam)
53fa956e  =>  collectTransactionFees(TimeswapV2PoolCollectParam)
55e305f3  =>  collect(uint256,uint256,address,address,address,uint256,uint256,uint256)
0150ca41  =>  mint(TimeswapV2PoolMintParam)
6da9a2a4  =>  mint(TimeswapV2PoolMintParam,uint96)
df52795d  =>  mint(TimeswapV2PoolMintParam,bool,uint96)
13576d77  =>  burn(TimeswapV2PoolBurnParam)
731d4f67  =>  burn(TimeswapV2PoolBurnParam,uint96)
bd4952bd  =>  burn(TimeswapV2PoolBurnParam,bool,uint96)
5d0ea1f5  =>  deleverage(TimeswapV2PoolDeleverageParam)
2e1b22ce  =>  deleverage(TimeswapV2PoolDeleverageParam,uint96)
cde7bf11  =>  deleverage(TimeswapV2PoolDeleverageParam,bool,uint96)
a97a4f78  =>  leverage(TimeswapV2PoolLeverageParam)
37aaeff8  =>  leverage(TimeswapV2PoolLeverageParam,uint96)
b8be2a15  =>  leverage(TimeswapV2PoolLeverageParam,bool,uint96)
c993c3fa  =>  rebalance(TimeswapV2PoolRebalanceParam)
```


### [G-27] Upgrade Solidity's optimizer

Make sure Solidity’s optimizer is enabled. It reduces gas costs. If you want to gas optimize for contract deployment (costs less to deploy a contract) then set the Solidity optimizer at a low number. If you want to optimize for run-time gas costs (when functions are called on a contract) then set the optimizer to a high number.

Set the optimization value higher than 800 in your hardhat.config.ts file.

3 results - 3 files:
```js
packages\v2-option\hardhat.config.ts:
  27: const config: HardhatUserConfig = {
  28:   paths: {
  29:     sources: "./src",
  30:   },
  31:   solidity: {
  32:     version: "0.8.8",
  33:     settings: {
  34:       optimizer: {
  35:         enabled: true,
  36:         runs: 200,
  37:       },
  38:     },
  39:   },
```

```js
packages\v2-pool\hardhat.config.ts:
  26: const config: HardhatUserConfig = {
  27:   paths: {
  28:     sources: "./src",
  29:   },
  30:   solidity: {
  31:     version: "0.8.8",
  32:     settings: {
  33:       optimizer: {
  34:         enabled: true,
  35:         runs: 200,
  36:       },
  37:     },
  38:   },
```

```js
packages\v2-token\hardhat.config.ts:
  26: const config: HardhatUserConfig = {
  27:   paths: {
  28:     sources: "./src",
  29:   },
  30:   solidity: {
  31:     version: "0.8.8",
  32:     settings: {
  33:       optimizer: {
  34:         enabled: true,
  35:         runs: 200,
  36:       },
  37:     },
  38:   },
```
### [G-28] Open the optimizer

Always use the Solidity optimizer to optimize gas costs. It's good practice to set the optimizer as high as possible until it no longer helps reduce gas costs in function calls. This is advisable since function calls are intended to be executed many more times than contract deployment, which only happens once.

In the light of this information, I suggest you to open the optimizer for ``v2-library``.
