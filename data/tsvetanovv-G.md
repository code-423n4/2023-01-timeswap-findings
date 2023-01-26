## GAS Optimisation List
| Number |Issues Details
|:--:|:-------|:--:|
|[GAS-01]| Use named returns for local variables where it is possible
|[GAS-02]| `x = x - y` costs less gas than `x -= y`, same for addition
|[GAS-03]| State variables only set in the constructor should be declared immutable
|[GAS-04]|Should use arguments instead of state variable
***

## [GAS-01] USE NAMED RETURNS FOR LOCAL VARIABLES WHERE IT IS POSSIBLE
```
packages/v2-library/src/BytesLib.sol:
12: function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {

packages/v2-pool/src/libraries/Duration.sol:
11: function init(uint256 duration) internal pure returns (uint96) {
```
***

## [GAS-02] `x = x - y` costs less gas than `x -= y`, same for addition
```
packages/v2-library/src/FullMath.sol
163: productA1 += (quotient1 * divisor)

In many places in packages/v2-pool/src/structs/LiquidityPosition.sol

packages/v2-pool/src/structs/Pool.sol:
361: if (long0Amount != 0) pool.long0Balance += long0Amount;
362: if (long1Amount != 0) pool.long1Balance += long1Amount;
365: pool.liquidity += liquidityAmount;
452: if (long0Amount != 0) pool.long0Balance -= long0Amount;
453: if (long1Amount != 0) pool.long1Balance -= long1Amount;
455: pool.liquidity -= liquidityAmount;
689: pool.long1Balance -= (long1Amount + longFees);
695: pool.long0Balance += long0Amount;
719: pool.long0Balance -= (long0Amount + longFees);
722: pool.long1Balance += long1Amount;

packages/v2-pool/src/libraries/ConstantProduct.sol:
149: if (isAdd) longAmount -= fees;
150: else shortAmount -= fees;
180: shortAmount -= fees;
211: fees = FeeCalculation.getFeesRemoval(longAmount, transactionFee);
212: longAmount -= fees;
244: amount -= fees;

packages/v2-token/src/base/ERC1155Enumerable.sol:
61: _idTotalSupply[id] += amount;
95: _idTotalSupply[id] -= amount;
117: _currentIndex[to] += 1;

In many places in packages/v2-option/src/structs/Option.sol

packages/v2-option/src/enums/Transaction.sol:
190: option.long0[msg.sender] -= token0AndLong0Amount;
191: option.long1[msg.sender] -= token1AndLong1Amount;
192: option.short[msg.sender] -= shortAmount;
237: if (param.isLong0ToLong1) option.long0[msg.sender] -= token0AndLong0Amount;
277: option.short[msg.sender] -= shortAmount;
```
You can replace all `-=` and `+=` occurrences to save gas
***

## [GAS-03] STATE VARIABLES ONLY SET IN THE CONSTRUCTOR SHOULD BE DECLARED IMMUTABLE
It allows setting contract-level variables at construction time which gets stored in code rather than storage.

#### USING IMMUTABLE ON VARIABLES THAT ARE ONLY SET IN THE CONSTRUCTOR AND NEVER AFTER
Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD. Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around 20 000 gas per variable) and replace the expensive storage-reading operations (around 2100 gas per reading) to a less expensive value reading (3 gas)

```
v2-pool/src/base/OwnableTwoSteps.sol:

14: address public override owner;
```
***

## [GAS-04] Should use arguments instead of state variable

```
v2-pool/src/base/OwnableTwoSteps.sol:

29:        pendingOwner = chosenPendingOwner;
31:        emit SetOwner(pendingOwner);
```
***