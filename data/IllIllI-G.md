## Summary

### Gas Optimizations
| |Issue|Instances|Total Gas Saved|
|-|:-|:-:|:-:|
| [G&#x2011;01] | Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`, where appropriate | 1 |  - |
| [G&#x2011;02] | Structs can be packed into fewer storage slots | 1 |  - |
| [G&#x2011;03] | Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas | 5 |  600 |
| [G&#x2011;04] | Avoid contract existence checks by using low level calls | 62 |  6200 |
| [G&#x2011;05] | State variables should be cached in stack variables rather than re-reading them from storage | 2 |  194 |
| [G&#x2011;06] | Optimize names to save gas | 12 |  264 |
| [G&#x2011;07] | Use a more recent version of solidity | 2 |  - |
| [G&#x2011;08] | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 19 |  - |

Total: 104 instances over 8 issues with **7258 gas** saved

Gas totals use lower bounds of ranges and count two iterations of each `for`-loop. All values above are runtime, not deployment, values; deployment values are listed in the individual issue descriptions. The table above as well as its gas numbers do not include any of the excluded findings.





## Gas Optimizations

### [G&#x2011;01]  Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`, where appropriate
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (**20000 gas**) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save **~42 gas per access** due to [not having to recalculate the key's keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

*There is 1 instance of this issue:*

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

52        mapping(uint256 => mapping(uint256 => uint96)) private reentrancyGuards;
53:       mapping(uint256 => mapping(uint256 => Pool)) private pools;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L52-L53

### [G&#x2011;02]  Structs can be packed into fewer storage slots
Each slot saved can avoid an extra Gsset (**20000 gas**) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings

*There is 1 instance of this issue:*

```solidity
File: packages/v2-token/src/structs/Position.sol

/// @audit Variable ordering with 4 slots instead of the current 5:
///           uint256(32):strike, uint256(32):maturity, address(20):token0, uint8(1):position, address(20):token1
12    struct TimeswapV2TokenPosition {
13        address token0;
14        address token1;
15        uint256 strike;
16        uint256 maturity;
17        TimeswapV2OptionPosition position;
18:   }

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/structs/Position.sol#L12-L18

### [G&#x2011;03]  Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. **Each iteration of this for-loop costs at least 60 gas** (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution. Note that even if an interface defines a function as having `memory` arguments, it's still valid for implementation contracs to use `calldata` arguments instead. 

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it's still more gass-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

Note that I've also flagged instances where the function is `public` but can be marked as `external` since it's not called by the contract, and cases where a constructor is involved

*There are 5 instances of this issue:*

```solidity
File: packages/v2-pool/src/structs/Pool.sol

/// @audit param
294       function mint(
295           Pool storage pool,
296           TimeswapV2PoolMintParam memory param,
297           uint96 blockTimestamp
298:      ) external returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit param
380       function burn(
381           Pool storage pool,
382           TimeswapV2PoolBurnParam memory param,
383           uint96 blockTimestamp
384:      ) external returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit param
469       function deleverage(
470           Pool storage pool,
471           TimeswapV2PoolDeleverageParam memory param,
472           uint256 transactionFee,
473           uint256 protocolFee,
474           uint96 blockTimestamp
475:      ) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit param
552       function leverage(
553           Pool storage pool,
554           TimeswapV2PoolLeverageParam memory param,
555           uint256 transactionFee,
556           uint256 protocolFee,
557           uint96 blockTimestamp
558:      ) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

/// @audit param
665:      function rebalance(Pool storage pool, TimeswapV2PoolRebalanceParam memory param, uint256 transactionFee, uint256 protocolFee) external returns (uint256 long0Amount, uint256 long1Amount) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L294-L298

### [G&#x2011;04]  Avoid contract existence checks by using low level calls
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

*There are 62 instances of this issue:*

```solidity
File: packages/v2-option/src/libraries/OptionFactory.sol

/// @audit get()
28:           optionPair = ITimeswapV2OptionFactory(optionFactory).get(token0, token1);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/libraries/OptionFactory.sol#L28

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

/// @audit parameter()
66:           (optionFactory, token0, token1) = ITimeswapV2OptionDeployer(msg.sender).parameter();

/// @audit balanceOf()
128:              IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,

/// @audit balanceOf()
129:              IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount

/// @audit timeswapV2OptionMintCallback()
133:          data = ITimeswapV2OptionMintCallback(msg.sender).timeswapV2OptionMintCallback(

/// @audit balanceOf()
145:          if (token0AndLong0Amount != 0) Error.checkEnough(IERC20(token0).balanceOf(address(this)), currentProcess.balance0Target);

/// @audit balanceOf()
148:          if (token1AndLong1Amount != 0) Error.checkEnough(IERC20(token1).balanceOf(address(this)), currentProcess.balance1Target);

/// @audit safeTransfer()
172:          if (token0AndLong0Amount != 0) IERC20(token0).safeTransfer(param.token0To, token0AndLong0Amount);

/// @audit safeTransfer()
175:          if (token1AndLong1Amount != 0) IERC20(token1).safeTransfer(param.token1To, token1AndLong1Amount);

/// @audit timeswapV2OptionBurnCallback()
179:              data = ITimeswapV2OptionBurnCallback(msg.sender).timeswapV2OptionBurnCallback(

/// @audit balanceOf()
/// @audit balanceOf()
215:              param.isLong0ToLong1 ? IERC20(token0).balanceOf(address(this)) - token0AndLong0Amount : IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,

/// @audit balanceOf()
/// @audit balanceOf()
216:              param.isLong0ToLong1 ? IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount : IERC20(token1).balanceOf(address(this)) - token1AndLong1Amount

/// @audit safeTransfer()
220:          IERC20(param.isLong0ToLong1 ? token0 : token1).safeTransfer(param.tokenTo, param.isLong0ToLong1 ? token0AndLong0Amount : token1AndLong1Amount);

/// @audit timeswapV2OptionSwapCallback()
223:          data = ITimeswapV2OptionSwapCallback(msg.sender).timeswapV2OptionSwapCallback(

/// @audit balanceOf()
235:          Error.checkEnough(IERC20(param.isLong0ToLong1 ? token1 : token0).balanceOf(address(this)), param.isLong0ToLong1 ? currentProcess.balance1Target : currentProcess.balance0Target);

/// @audit safeTransfer()
259:          if (token0Amount != 0) IERC20(token0).safeTransfer(param.token0To, token0Amount);

/// @audit safeTransfer()
262:          if (token1Amount != 0) IERC20(token1).safeTransfer(param.token1To, token1Amount);

/// @audit timeswapV2OptionCollectCallback()
266:              data = ITimeswapV2OptionCollectCallback(msg.sender).timeswapV2OptionCollectCallback(

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L66

```solidity
File: packages/v2-pool/src/libraries/PoolFactory.sol

/// @audit get()
32:           poolPair = ITimeswapV2PoolFactory(poolFactory).get(optionPair);

/// @audit get()
46:           poolPair = ITimeswapV2PoolFactory(poolFactory).get(optionPair);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L32

```solidity
File: packages/v2-pool/src/structs/Pool.sol

/// @audit timeswapV2PoolMintChoiceCallback()
349:          (long0Amount, long1Amount, data) = ITimeswapV2PoolMintCallback(msg.sender).timeswapV2PoolMintChoiceCallback(

/// @audit timeswapV2PoolBurnChoiceCallback()
438:          (long0Amount, long1Amount, data) = ITimeswapV2PoolBurnCallback(msg.sender).timeswapV2PoolBurnChoiceCallback(

/// @audit timeswapV2PoolDeleverageChoiceCallback()
532:          (long0Amount, long1Amount, data) = ITimeswapV2PoolDeleverageCallback(msg.sender).timeswapV2PoolDeleverageChoiceCallback(

/// @audit timeswapV2PoolLeverageChoiceCallback()
615:              (long0Amount, long1Amount, data) = ITimeswapV2PoolLeverageCallback(msg.sender).timeswapV2PoolLeverageChoiceCallback(

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L349

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

/// @audit parameter()
78:           (poolFactory, optionPair, transactionFee, protocolFee) = ITimeswapV2PoolDeployer(msg.sender).parameter();

/// @audit owner()
189:          ITimeswapV2PoolFactory(poolFactory).owner().checkIfOwner();

/// @audit positionOf()
267:          if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;

/// @audit positionOf()
271:          if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;

/// @audit positionOf()
274:          uint256 shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;

/// @audit timeswapV2PoolMintCallback()
277:          data = ITimeswapV2PoolMintCallback(msg.sender).timeswapV2PoolMintCallback(

/// @audit positionOf()
293:          if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);

/// @audit positionOf()
295:          if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);

/// @audit positionOf()
297:          Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);

/// @audit timeswapV2PoolBurnCallback()
343:          data = ITimeswapV2PoolBurn2Callback(msg.sender).timeswapV2PoolBurnCallback(

/// @audit positionOf()
393:          if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;

/// @audit positionOf()
397:          if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;

/// @audit timeswapV2PoolDeleverageCallback()
403:          data = ITimeswapV2PoolDeleverageCallback(msg.sender).timeswapV2PoolDeleverageCallback(

/// @audit positionOf()
411:          if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);

/// @audit positionOf()
413:          if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);

/// @audit positionOf()
444:          uint256 balanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;

/// @audit timeswapV2PoolLeverageCallback()
453:          data = ITimeswapV2PoolLeverageCallback(msg.sender).timeswapV2PoolLeverageCallback(

/// @audit positionOf()
461:          Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), balanceTarget);

/// @audit positionOf()
479:          uint256 balanceTarget = ITimeswapV2Option(optionPair).positionOf(

/// @audit timeswapV2PoolRebalanceCallback()
497:          data = ITimeswapV2PoolRebalanceCallback(msg.sender).timeswapV2PoolRebalanceCallback(

/// @audit positionOf()
511:              ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), param.isLong0ToLong1 ? TimeswapV2OptionPosition.Long0 : TimeswapV2OptionPosition.Long1),

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L78

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

/// @audit liquidityOf()
125:          uint160 liquidityBalanceTarget = ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)) + param.liquidityAmount;

/// @audit timeswapV2LiquidityTokenMintCallback()
131:          data = ITimeswapV2LiquidityTokenMintCallback(msg.sender).timeswapV2LiquidityTokenMintCallback(

/// @audit liquidityOf()
143:          Error.checkEnough(ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)), liquidityBalanceTarget);

/// @audit timeswapV2LiquidityTokenBurnCallback()
163:              data = ITimeswapV2LiquidityTokenBurnCallback(msg.sender).timeswapV2LiquidityTokenBurnCallback(

/// @audit timeswapV2LiquidityTokenCollectCallback()
202:              data = ITimeswapV2LiquidityTokenCollectCallback(msg.sender).timeswapV2LiquidityTokenCollectCallback(

/// @audit feeGrowth()
246:                  (long0FeeGrowth, long1FeeGrowth, shortFeeGrowth) = ITimeswapV2Pool(poolPair).feeGrowth(timeswapV2LiquidityTokenPosition.strike, timeswapV2LiquidityTokenPosition.maturity);

/// @audit feeGrowth()
272:              (long0FeeGrowth, long1FeeGrowth, shortFeeGrowth) = ITimeswapV2Pool(poolPair).feeGrowth(timeswapV2LiquidityTokenPosition.strike, timeswapV2LiquidityTokenPosition.maturity);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L125

```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

/// @audit positionOf()
87:               long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + param.long0Amount;

/// @audit positionOf()
117:              long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + param.long1Amount;

/// @audit positionOf()
146:              shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + param.shortAmount;

/// @audit timeswapV2TokenMintCallback()
172:          data = ITimeswapV2TokenMintCallback(msg.sender).timeswapV2TokenMintCallback(

/// @audit positionOf()
186:          if (param.long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);

/// @audit positionOf()
189:          if (param.long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);

/// @audit positionOf()
192:          if (param.shortAmount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);

/// @audit timeswapV2TokenBurnCallback()
216:              data = ITimeswapV2TokenBurnCallback(msg.sender).timeswapV2TokenBurnCallback(

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L87

### [G&#x2011;05]  State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (**100 gas**) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*There are 2 instances of this issue:*

```solidity
File: packages/v2-pool/src/base/OwnableTwoSteps.sol

/// @audit owner on line 24
27:           chosenPendingOwner.checkIfAlreadyOwner(owner);

/// @audit pendingOwner on line 29
31:           emit SetOwner(pendingOwner);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L27

### [G&#x2011;06]  Optimize names to save gas
`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

*There are 12 instances of this issue:*

```solidity
File: packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol

/// @audit get(), getByIndex(), numberOfPairs(), create()
6:    interface ITimeswapV2OptionFactory {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L6

```solidity
File: packages/v2-option/src/interfaces/ITimeswapV2Option.sol

/// @audit optionFactory(), token0(), token1(), getByIndex(), numberOfOptions(), totalPosition(), positionOf(), transferPosition(), mint(), burn(), swap(), collect()
11:   interface ITimeswapV2Option {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L11

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol

/// @audit timeswapV2PoolDeleverageChoiceCallback(), timeswapV2PoolDeleverageCallback()
7:    interface ITimeswapV2PoolDeleverageCallback {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L7

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol

/// @audit timeswapV2PoolLeverageChoiceCallback(), timeswapV2PoolLeverageCallback()
7:    interface ITimeswapV2PoolLeverageCallback {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol#L7

```solidity
File: packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol

/// @audit timeswapV2PoolMintChoiceCallback(), timeswapV2PoolMintCallback()
7:    interface ITimeswapV2PoolMintCallback {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L7

```solidity
File: packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol

/// @audit pendingOwner(), setPendingOwner(), acceptOwner()
4:    interface IOwnableTwoSteps {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol#L4

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol

/// @audit transactionFee(), protocolFee(), get(), getByIndex(), numberOfPairs(), create()
8:    interface ITimeswapV2PoolFactory is IOwnableTwoSteps {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L8

```solidity
File: packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol

/// @audit poolFactory(), optionPair(), transactionFee(), protocolFee(), getByIndex(), numberOfPools(), totalLiquidity(), sqrtInterestRate(), liquidityOf(), feeGrowth(), feesEarnedOf(), protocolFeesEarned(), totalLongBalance(), totalLongBalanceAdjustFees(), totalPositions(), transferLiquidity(), transferFees(), initialize(), collectProtocolFees(), collectTransactionFees(), mint(), mint(), burn(), burn(), deleverage(), deleverage(), leverage(), leverage(), rebalance()
9:    interface ITimeswapV2Pool {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L9

```solidity
File: packages/v2-pool/src/structs/Pool.sol

/// @audit feeGrowth(), feesEarnedOf(), protocolFeesEarned(), totalLongBalanceAdjustFees(), totalPositions(), transferLiquidity(), transferFees(), initialize(), collectProtocolFees(), collectTransactionFees(), mint(), burn(), deleverage(), leverage(), rebalance()
56:   library PoolLibrary {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L56

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol

/// @audit optionFactory(), poolFactory(), positionOf(), transferTokenPositionFrom(), transferFeesFrom(), mint(), burn(), collect()
10:   interface ITimeswapV2LiquidityToken is IERC1155Enumerable {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L10

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2Token.sol

/// @audit optionFactory(), positionOf(), transferTokenPositionFrom(), mint(), burn()
11:   interface ITimeswapV2Token is IERC1155 {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L11

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

/// @audit positionOf(), transferTokenPositionFrom(), mint(), burn(), collect()
27:   contract TimeswapV2LiquidityToken is ITimeswapV2LiquidityToken, ERC1155Enumerable {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L27

### [G&#x2011;07]  Use a more recent version of solidity
Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

*There are 2 instances of this issue:*

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

4:    pragma solidity ^0.8.0;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L4

```solidity
File: packages/v2-token/src/interfaces/IERC1155Enumerable.sol

4:    pragma solidity ^0.8.0;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L4

### [G&#x2011;08]  Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead
> When using elements that are smaller than 32 bytes, your contractâ€™s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra [**22-28 gas**](https://gist.github.com/IllIllI000/9388d20c70f9a4632eb3ca7836f54977) (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

*There are 19 instances of this issue:*

```solidity
File: packages/v2-library/src/SafeCast.sol

/// @audit uint16 result
20:           result = uint16(value);

/// @audit uint96 result
29:           result = uint96(value);

/// @audit uint160 result
38:           result = uint160(value);

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/SafeCast.sol#L20

```solidity
File: packages/v2-pool/src/libraries/ConstantProduct.sol

/// @audit uint160 liquidityAmount
67:           liquidityAmount = getLiquidityGivenLong(rate, longAmount, !isAdd);

/// @audit uint160 liquidityAmount
82:           liquidityAmount = getLiquidityGivenShort(rate, shortAmount, duration, !isAdd);

/// @audit uint160 liquidityAmount
104:          liquidityAmount = getLiquidityGivenLong(rate, amount, !isAdd);

/// @audit uint160 liquidityAmount
110:              liquidityAmount = getLiquidityGivenShort(rate, amount, duration, !isAdd);

/// @audit uint160 newRate
142:          newRate = isAdd ? rate + deltaRate : rate - deltaRate;

/// @audit uint160 newRate
174:          newRate = getNewSqrtInterestRateGivenLong(liquidity, rate, longAmount + (isAdd ? 0 : fees), isAdd);

/// @audit uint160 newRate
241:          else newRate = getNewSqrtInterestRateGivenLong(liquidity, rate, amount, false);

/// @audit uint160 deltaRate
316:          deltaRate = FullMath.mulDiv(shortAmount, uint256(1) << 192, denominator, !isAdd).toUint160();

/// @audit uint160 newRate
318:          if (isAdd) newRate = rate + deltaRate;

/// @audit uint160 newRate
319:          else if (rate > deltaRate) newRate = rate - deltaRate;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ConstantProduct.sol#L67

```solidity
File: packages/v2-pool/src/libraries/DurationCalculation.sol

/// @audit uint96 duration
19:           duration = uint256(blockTimestamp).unsafeSub(lastTimestamp).toUint96();

/// @audit uint96 duration
27:           duration = maturity.unsafeSub(uint256(blockTimestamp)).toUint96();

/// @audit uint96 duration
35:           duration = maturity.unsafeSub(lastTimestamp).toUint96();

/// @audit uint96 duration
44:           duration = maturity.min(blockTimestamp).unsafeSub(lastTimestamp).toUint96();

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L19

```solidity
File: packages/v2-pool/src/structs/Pool.sol

/// @audit uint160 liquidityAmount
308:                  liquidityAmount = param.delta.toUint160(),

/// @audit uint160 liquidityAmount
398:                  liquidityAmount = param.delta.toUint160(),

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L308


___

## Excluded findings
These findings are excluded from awards calculations because there are publicly-available automated tools that find them. The valid ones appear here for completeness

## Summary

### Gas Optimizations
| |Issue|Instances|Total Gas Saved|
|-|:-|:-:|:-:|
| [G&#x2011;01] | `<array>.length` should not be looked up in every loop of a `for`-loop | 4 |  12 |
| [G&#x2011;02] | Using `bool`s for storage incurs overhead | 1 |  17100 |
| [G&#x2011;03] | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) | 2 |  10 |
| [G&#x2011;04] | Use custom errors rather than `revert()`/`require()` strings to save gas | 2 |  - |

Total: 9 instances over 4 issues with **17122 gas** saved

Gas totals use lower bounds of ranges and count two iterations of each `for`-loop. All values above are runtime, not deployment, values; deployment values are listed in the individual issue descriptions. The table above as well as its gas numbers do not include any of the excluded findings.





## Gas Optimizations

### [G&#x2011;01]  `<array>.length` should not be looked up in every loop of a `for`-loop
The overheads outlined below are _PER LOOP_, excluding the first loop
* storage arrays incur a Gwarmaccess (**100 gas**)
* memory arrays use `MLOAD` (**3 gas**)
* calldata arrays use `CALLDATALOAD` (**3 gas**)

Caching the length changes each of these to a `DUP<N>` (**3 gas**), and gets rid of the extra `DUP<N>` needed to store the stack offset

*There are 4 instances of this issue:*

```solidity
File: packages/v2-option/src/structs/Process.sol

/// @audit (valid but excluded finding)
34:           for (uint256 i; i < processing.length; ) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Process.sol#L34

```solidity
File: packages/v2-token/src/base/ERC1155Enumerable.sol

/// @audit (valid but excluded finding)
48:           for (uint256 i; i < ids.length; ) {

/// @audit (valid but excluded finding)
82:           for (uint256 i; i < ids.length; ) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L48

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

/// @audit (valid but excluded finding)
227:          for (uint256 i; i < ids.length; ) {

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L227

### [G&#x2011;02]  Using `bool`s for storage incurs overhead
```solidity
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.
```
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27
Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (**[100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)**) for the extra SLOAD, and to avoid Gsset (**20000 gas**) when changing from `false` to `true`, after having been `true` in the past

*There is 1 instance of this issue:*

```solidity
File: packages/v2-option/src/TimeswapV2Option.sol

/// @audit (valid but excluded finding)
53:       mapping(uint256 => mapping(uint256 => bool)) private hasInteracted;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L53

### [G&#x2011;03]  `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)
Saves **5 gas per loop**

*There are 2 instances of this issue:*

```solidity
File: packages/v2-library/src/FullMath.sol

/// @audit (valid but excluded finding)
167:                      quotient1++;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/FullMath.sol#L167

```solidity
File: packages/v2-option/src/structs/Process.sol

/// @audit (valid but excluded finding)
42:                   i++;

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Process.sol#L42

### [G&#x2011;04]  Use custom errors rather than `revert()`/`require()` strings to save gas
Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

*There are 2 instances of this issue:*

```solidity
File: packages/v2-library/src/BytesLib.sol

/// @audit (valid but excluded finding)
13:           require(_length + 31 >= _length, "slice_overflow");

/// @audit (valid but excluded finding)
14:           require(_bytes.length >= _start + _length, "slice_outOfBounds");

```
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-library/src/BytesLib.sol#L13
