## Summary
### Low Risk Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
|[L-01]| Some tokens can have 2 addresses, so should be done check other require| 1 |
|[L-02]| ` TransferFees ` event is missing parameters | 1 |
|[L-03]| Missing Event for  initialize| 8 |
|[L-04]| Use OpenZeppelin’s **Ownable2Step instead of inheriting from your own implementation for a 2 step ownership transfer| 1 |
|[L-05]| Stack too deep when compiling| 1 |
|[L-06]| Need Fuzzing test | 10 |
|[L-07]|Omissions in Events | 1 |
|[L-08]| Lack of control to assign 0 values in the value assignments of critical state variables in the constructor| 1 |
|[L-09]| The project has the risk of getting a Vampire Attack due to the fixed fee rate|  |
|[L-10]| Use the latest updated version of OpenZeppelin dependencies| 3|
|[L-11]| Add parameter to Event-Emit for critical function| 1 |
|[L-12]| Missing ReEntrancy Guard to `swap` function| 1 |
|[L-13]| Use UniswapV3’s `mulDiv` function for rigorous validation| 1 |
|[L-14]| Storage Write Removal Bug On Conditional Early Termination| 1 |
|[L-15]| Critical technical faulty definition in the documents|  |

Total 15 issues


### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [NC-01]|Remove Unused Codes|2|
| [NC-02] |Insufficient coverage  |1|
| [NC-03] |NatSpec comments should be increased in contracts| All Contracts |
| [NC-04] |`Function writing` that does not comply with the `Solidity Style Guide`| All Contracts |
| [NC-05] |Add a timelock to critical functions | 1|
| [NC-06] |Include return parameters in NatSpec comments  | All Contracts |
| [NC-07] |Tokens accidentally sent to the contract cannot be recovered| 1 |
| [NC-08] |Assembly Codes Specific – Should Have Comments | 10 |
| [NC-09] |Take advantage of Custom Error's return value property| 38 |
| [NC-10] |Repeated validation logic can be converted into a function modifier| 37  |
| [NC-11] |For modern and more readable code; update import usages| 1 |
| [NC-12] |Use a more recent version of Solidity |All Contracts  |
| [NC-13] |For functions, follow Solidity standard naming conventions (internal function style rule)| 139 |
| [NC-14] |Floating pragma| 8 |
| [NC-15] |Project Upgrade and Stop Scenario should be |  |
| [NC-16] |Use descriptive names for Contracts and Libraries| All Contracts |
| [NC-17] |Use SMTChecker|  |
| [NC-18] |Remove the codes used in the test area in the actual codes | 1 |
| [NC-19] |Add NatSpec Mapping comment| 29 |
| [NC-20] |There is no need to cast a variable that is already an address, such as address(x)| 1 |
| [NC-21] |Lines are too long| 67 |
| [NC-22] |Assembly Codes Specific – Should Have Comments| 2 |
| [NC-23] |Move owner checks to a modifier| 2 |

Total 23 issues

### [L-01] Some tokens can have 2 addresses, so should be done check other require

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionPair.sol#L29-L31


### Impact

In the `OptionPair.checkCorrectFormat()` and `OptionPair.checkDoesNotExist()` functions, the Pair consisting of address token0 and token1 is checked for uniqueness;

```solidity
     function checkCorrectFormat(address token0, address token1) internal pure {
         if (token0 >= token1) revert InvalidOptionPair(token0, token1);
     }
   
     /// @dev Check if the pair already existed.
     /// @notice Reverts if the pair is not a zero address.
     /// @param token0 The first ERC20 token address of the pair.
     /// @param token1 The second ERC20 token address of the pair.
     /// @param optionPair The address of the existed Pair contract.
     function checkDoesNotExist(address token0, address token1, address optionPair) internal pure {
         if (optionPair != address(0)) revert OptionPairAlreadyExisted(token0, token1, optionPair);
     }

```

But some tokens have 2 addresses through which they can be interacted with.

https://medium.com/chainsecurity/trueusd-compound-vulnerability-bc5b696d29e2

One of the two addresses can be used when interacting with the relevant token, so a check should be added additionally to prevent tokens that can be interacted with address 2




### Recommended Mitigation Steps

The name and symbol of Token0 and Token1 can be stored in a mapping and a require can be added to check with every existing Token0 and Token1



### [L-02] ` TransferFees ` event is missing parameters


The ` TimeswapV2LiquidityToken.sol` contract has very important function; ` transferFeesFrom ` 


However, only amounts are published in emits, whereas msg.sender must be specified in every transaction, a contract or web page listening to events cannot react to users, emit does not serve the purpose.
Basically, this event cannot be used


```solidity
packages/v2-token/src/TimeswapV2LiquidityToken.sol:
  74      /// @inheritdoc ITimeswapV2LiquidityToken
  75:     function transferFeesFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external override {
  76:         if (from == address(0)) Error.zeroAddress();
  77:         if (to == address(0)) Error.zeroAddress();
  78: 
  79:         if (!isApprovedForAll(from, msg.sender)) revert NotApprovedToTransferFees();
  80: 
  81:         uint256 id = _timeswapV2LiquidityTokenPositionIds[position.toKey()];
  82: 
  83:         if (long0Fees != 0 || long1Fees != 0 || shortFees != 0) _addTokenEnumeration(from, to, id, 0);
  84: 
  85:         // add/mint the fees for the new user
  86:         _feesPositions[id][to].mint(long0Fees, long1Fees, shortFees);
  87: 
  88:         _updateFeesPositions(from, to, id);
  89: 
  90:         // remove/burn the fees
  91:         _feesPositions[id][from].burn(long0Fees, long1Fees, shortFees);
  92: 
  93:         if (long0Fees != 0 || long1Fees != 0 || shortFees != 0) _removeTokenEnumeration(from, to, id, 0);
  94: 
  95:         emit TransferFees(from, to, position, long0Fees, long1Fees, shortFees);
  96:     }

```



### Recommended Mitigation Steps
add `msg.sender` parameter in event-emit

### [L-03] Missing Event for  initialize

**Context:**
```solidity

8 results - 8 files

packages/v2-option/src/NoDelegateCall.sol:
  18  
  19:     constructor() {
  20          // Immutables are computed in the init code of the contract, and then inlined into the deployed bytecode.

packages/v2-option/src/TimeswapV2Option.sol:
  64  
  65:     constructor() NoDelegateCall() {
  66          (optionFactory, token0, token1) = ITimeswapV2OptionDeployer(msg.sender).parameter();

packages/v2-pool/src/NoDelegateCall.sol:
  18  
  19:     constructor() {
  20          // Immutables are computed in the init code of the contract, and then inlined into the deployed bytecode.

packages/v2-pool/src/TimeswapV2Pool.sol:
  76  
  77:     constructor() NoDelegateCall() {
  78          (poolFactory, optionPair, transactionFee, protocolFee) = ITimeswapV2PoolDeployer(msg.sender).parameter();

packages/v2-pool/src/TimeswapV2PoolFactory.sol:
  36  
  37:     constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) {
  38          if (chosenTransactionFee > type(uint16).max) revert IncorrectFeeInitialization(chosenTransactionFee);

packages/v2-pool/src/base/OwnableTwoSteps.sol:
  17  
  18:     constructor(address chosenOwner) {
  19          owner = chosenOwner;

packages/v2-token/src/TimeswapV2LiquidityToken.sol:
  35  
  36:     constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {
  37          optionFactory = chosenOptionFactory;

packages/v2-token/src/TimeswapV2Token.sol:
  40  
  41:     constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {
  42          optionFactory = chosenOptionFactory;

```

**Description:**
Events help non-contract tools to track changes, and events prevent users from being surprised by changes
Issuing event-emit during initialization is a detail that many projects skip

**Recommendation:**
Add Event-Emit

### [L-04] Use OpenZeppelin’s **Ownable2Step instead of inheriting from your own implementation for a 2 step ownership transfer

Project makes secure Owner change with OwnableTwoStep

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol

However, I recommend using the battle-tested, safe and gas-optimized Openzeppelin in Ownable2Step.sol;

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol


**Recommendation:**
Use OpenZeppelin Ownable2Step.sol



### [L-05] Stack too deep when compiling

The project cannot be compiled due to the "stack too deep" error.

The “stack too deep” error is a limitation of the current code generator. The EVM stack only has 16 slots and that’s sometimes not enough to fit all the local variables, parameters and/or return variables. The solution is to move some of them to memory, which is more expensive but at least makes your code compile.

```js
1 result - 1 file

README.md:
  157:   - (`forge coverage` currently throws a "stack too deep" error on large codebases)
```


CompilerError: Stack too deep. Try compiling with `--via-ir` (cli) or the equivalent `viaIR: true` (standard JSON) while enabling the optimizer. Otherwise, try removing local variables. When compiling inline assembly: Variable headStart is 1 slot(s) too deep inside the stack. Stack too deep. Try compiling with `--via-ir` (cli) or the equivalent `viaIR: true` (standard JSON) while enabling the optimizer. Otherwise, try removing local variables.


ref:https://forum.openzeppelin.com/t/stack-too-deep-when-compiling-inline-assembly/11391/6

### [L-06] Need Fuzzing test

**Context:**

```solidity

10 results 


packages/v2-library/src/Math.sol:
  14      function unsafeAdd(uint256 addend1, uint256 addend2) internal pure returns (uint256 sum) {
  15:         unchecked {
  16              sum = addend1 + addend2;

  25      function unsafeSub(uint256 minuend, uint256 subtrahend) internal pure returns (uint256 difference) {
  26:         unchecked {
  27              difference = minuend - subtrahend;

  36      function unsafeMul(uint256 multiplicand, uint256 multiplier) internal pure returns (uint256 product) {
  37:         unchecked {
  38              product = multiplicand * multiplier;

  71          if (value == 0) return 0;
  72:         unchecked {
  73              uint256 estimate = (value + 1) >> 1;

packages/v2-option/src/structs/Process.sol:
  40  
  41:             unchecked {
  42                  i++;

packages/v2-pool/src/libraries/ConstantProduct.sol:
  278          uint256 numerator;
  279:         unchecked {
  280              numerator = uint256(liquidity) << 96;

  330          uint256 numerator;
  331:         unchecked {
  332              numerator = uint256(liquidity) << 96;

packages/v2-token/src/TimeswapV2LiquidityToken.sol:
  229  
  230:             unchecked {
  231                  ++i;

packages/v2-token/src/base/ERC1155Enumerable.sol:
  50  
  51:             unchecked {
  52                  ++i;

  84  
  85:             unchecked {
  86                  ++i;

```

**Description:**
In total 10 unchecked are used, the functions used are critical. For this reason, there must be fuzzing tests in the tests of the project. Not seen in tests.

**Recommendation:**
Use should fuzzing test like Echidna.

As Alberto Cuesta Canada said:
_Fuzzing is not easy, the tools are rough, and the math is hard, but it is worth it. Fuzzing gives me a level of confidence in my smart contracts that I didn’t have before. Relying just on unit testing anymore and poking around in a testnet seems reckless now._

https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05

### [L-07] Omissions in Events

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

The events should include the new value and old value where possible:

```diff

packages/v2-pool/src/base/OwnableTwoSteps.sol:

 address public override owner;
 address public override pendingOwner;
+ address public override OldOwner;


  22      /// @inheritdoc IOwnableTwoSteps
  23:     function setPendingOwner(address chosenPendingOwner) external override {
  24:         Ownership.checkIfOwner(owner);
  25: 
  26:         if (chosenPendingOwner == address(0)) Error.zeroAddress();
  27:         chosenPendingOwner.checkIfAlreadyOwner(owner);
  28: 
  29:         pendingOwner = chosenPendingOwner;
  30: 
- 31:         emit SetOwner(pendingOwner);
+ 31:         emit SetOwner(OldOwner, pendingOwner);
  32:     }
```
### [L-08] Lack of control to assign 0 values in the value assignments of critical state variables in the constructor


`chosenTransactionFee` and `chosenProtocolFee` variables are critical uint256 values. So should be check 0 value in constructor
If the fees are 0 by mistake, even for a while, it will cause a loss to the project.

```solidity
packages/v2-pool/src/TimeswapV2PoolFactory.sol:
  36  
  37:     constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) {
	....//codes
  41:         transactionFee = chosenTransactionFee;
  42:         protocolFee = chosenProtocolFee;
  43:     }

```
### [L-09] The project has the risk of getting a Vampire Attack due to the fixed fee rate.


```solidity
packages/v2-pool/src/TimeswapV2PoolFactory.sol:
  36  
  37:     constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner) {
  38:         if (chosenTransactionFee > type(uint16).max) revert IncorrectFeeInitialization(chosenTransactionFee);
  39:         if (chosenProtocolFee > type(uint16).max) revert IncorrectFeeInitialization(chosenProtocolFee);
  40: 
  41:         transactionFee = chosenTransactionFee;
  42:         protocolFee = chosenProtocolFee;
  43:     }
```

What is a Vampire Attack in Crypto?
https://medium.com/@buraktahtacioglu/looksrare-vampire-attack-on-opensea-blockchain-roadmap-29bd51753a64
https://omerkeman.medium.com/what-is-a-vampire-attack-in-crypto-fdfc5e1fc5fc


### Recommended Mitigation Steps

Instead of a fixed fee rate, this problem can be solved in two ways;
1- Function is added so that the rate of Fee can be updated by an authorized owner
2- Core Contract can be made upgradable (A different architecture is required for this)
3- The commission rate should be fixed with a certain upper rate so that users will not have a trust problem.


Uniswap v3  solved this issue as follows;
```solidity
 /// @inheritdoc IUniswapV3PoolOwnerActions
    function setFeeProtocol(uint8 feeProtocol0, uint8 feeProtocol1) external override lock onlyFactoryOwner {
        require(
            (feeProtocol0 == 0 || (feeProtocol0 >= 4 && feeProtocol0 <= 10)) &&
                (feeProtocol1 == 0 || (feeProtocol1 >= 4 && feeProtocol1 <= 10))
        );
        uint8 feeProtocolOld = slot0.feeProtocol;
        slot0.feeProtocol = feeProtocol0 + (feeProtocol1 << 4);
        emit SetFeeProtocol(feeProtocolOld % 16, feeProtocolOld >> 4, feeProtocol0, feeProtocol1);
    }
```

NFTX solved this issue as follows;

https://github.com/NFTX-project/nftx-protocol-v2/blob/master/contracts/solidity/NFTXVaultFactoryUpgradeable.sol#L77-L97

```solidity
 function setFactoryFees(
        uint256 mintFee, 
        uint256 randomRedeemFee, 
        uint256 targetRedeemFee,
        uint256 randomSwapFee, 
        uint256 targetSwapFee
    ) public onlyOwner virtual override {
        require(mintFee <= 0.5 ether, "Cannot > 0.5 ether");
        require(randomRedeemFee <= 0.5 ether, "Cannot > 0.5 ether");
        require(targetRedeemFee <= 0.5 ether, "Cannot > 0.5 ether");
        require(randomSwapFee <= 0.5 ether, "Cannot > 0.5 ether");
        require(targetSwapFee <= 0.5 ether, "Cannot > 0.5 ether");

        factoryMintFee = uint64(mintFee);
        factoryRandomRedeemFee = uint64(randomRedeemFee);
        factoryTargetRedeemFee = uint64(targetRedeemFee);
        factoryRandomSwapFee = uint64(randomSwapFee);
        factoryTargetSwapFee = uint64(targetSwapFee);

        emit UpdateFactoryFees(mintFee, randomRedeemFee, targetRedeemFee, randomSwapFee, targetSwapFee);
    }

```

### [L-10] Use the latest updated version of OpenZeppelin dependencies

```js
3 results 

packages/v2-library/package.json:
  39    "dependencies": {
  40:     "@openzeppelin/contracts": "^4.7.2",

packages/v2-pool/package.json:
  40    "dependencies": {
  41:     "@openzeppelin/contracts": "^4.8.0",

packages/v2-token/package.json:
  39    "dependencies": {
  40:     "@openzeppelin/contracts": "^4.7.2",

```

### Proof Of Concept

https://github.com/OpenZeppelin/openzeppelin-contracts/releases/


### Recommended Mitigation Steps
Upgrade `OpenZeppelin` to version 4.8.1

### [L-11] Add parameter to Event-Emit for critical function

Some event-emit description hasn’t parameter. Add to parameter  for front-end website or client app , they can has that something has happened on the blockchain.

Events with no old value;

```solidity

packages/v2-option/src/TimeswapV2OptionDeployer.sol:
  31      /// @return optionPair The address of the newly deployed TimeswapV2Option contract.
  32:     function deploy(address optionFactory, address token0, address token1) internal returns (address optionPair) {
  33:         parameter = Parameter({optionFactory: optionFactory, token0: token0, token1: token1});
  34: 
  35:         optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
  36: 
  37:         // save gas.
  38:         delete parameter;
  39:     }

```

### [L-12] Missing ReEntrancy Guard to `swap` function



### Impact

There is no re-entry risk on true ERC-20 tokens that work according to the spec (i.e. audited, etc.).

However you can write a malicious ERC-20 with custom safetransferFrom() or approve() that have re-entrancy hooks to attack a target.

Furthermore ERC-777 is backwards compatible token standard with ERC-20 standard. ERC-777 has better usability, but it has transfer hooks that can cause re-entrancy.


Vulnerability Detail
ERC20 generally doesn't result in reentrancy, however ERC777 tokens can and they can maskerade as ERC20. So if a contract interacts with unknown ERC20 tokens it is better to be safe and consider that transfers can create reentrancy problems.

Must be re-entrancy guard to below function;

```solidity
packages/v2-option/src/TimeswapV2Option.sol:
  197      /// @inheritdoc ITimeswapV2Option
  198:     function swap(TimeswapV2OptionSwapParam calldata param) external override noDelegateCall returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, bytes memory data) {
  199:         if (!hasInteracted[param.strike][param.maturity]) Error.inactiveOptionChoice(param.strike, param.maturity);
  200:         ParamLibrary.check(param, blockTimestamp());
  201: 
  202:         Option storage option = options[param.strike][param.maturity];
  203: 
  204:         // does main swap logic calculation
  205:         (token0AndLong0Amount, token1AndLong1Amount) = option.swap(param.strike, param.longTo, param.isLong0ToLong1, param.transaction, param.amount);
  206: 
  207:         // update token0 and token1 balance target for any previous concurrent option transactions.
  208:         processing.updateProcess(token0AndLong0Amount, token1AndLong1Amount, !param.isLong0ToLong1, param.isLong0ToLong1);
  209: 
  210:         // add a new process
  211:         // stores the token0 and token1 balance target required from the msg.sender to achieve.
  212:         Process storage currentProcess = (processing.push() = Process(
  213:             param.strike,
  214:             param.maturity,
  215:             param.isLong0ToLong1 ? IERC20(token0).balanceOf(address(this)) - token0AndLong0Amount : IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
  216:             param.isLong0ToLong1 ? IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount : IERC20(token1).balanceOf(address(this)) - token1AndLong1Amount
  217:         ));
  218: 
  219:         // transfer token to recipient.
  220:         IERC20(param.isLong0ToLong1 ? token0 : token1).safeTransfer(param.tokenTo, param.isLong0ToLong1 ? token0AndLong0Amount : token1AndLong1Amount);
  221: 
  222:         // ask the msg.sender to transfer token0 or token1 to this contract.
  223:         data = ITimeswapV2OptionSwapCallback(msg.sender).timeswapV2OptionSwapCallback(
  224:             TimeswapV2OptionSwapCallbackParam({
  225:                 strike: param.strike,
  226:                 maturity: param.maturity,
  227:                 isLong0ToLong1: param.isLong0ToLong1,
  228:                 token0AndLong0Amount: token0AndLong0Amount,
  229:                 token1AndLong1Amount: token1AndLong1Amount,
  230:                 data: param.data
  231:             })
  232:         );
  233: 
  234:         // check if the token0 or token1 balance target is achieved.
  235:         Error.checkEnough(IERC20(param.isLong0ToLong1 ? token1 : token0).balanceOf(address(this)), param.isLong0ToLong1 ? currentProcess.balance1Target : currentProcess.balance0Target);
  236: 
  237:         if (param.isLong0ToLong1) option.long0[msg.sender] -= token0AndLong0Amount;
  238:         else option.long1[msg.sender] -= token1AndLong1Amount;
  239: 
  240:         // finish the process.
  241:         processing.pop();
  242: 
  243:         emit Swap(param.strike, param.maturity, msg.sender, param.tokenTo, param.longTo, param.isLong0ToLong1, token0AndLong0Amount, token1AndLong1Amount);
  244:     }
  245 

```


Although reentrancy attack is considered quite old over the past two years, there have been cases such as:

Uniswap/LendfMe hacks (2020) ($25 mln, attacked by a hacker using a reentrancy)

The BurgerSwap hack (May 2021) ( $7.2 million because of a fake token contract and a reentrancy exploit.)

The SURGEBNB hack (August 2021) ($4 million seems to be a reentrancy-based price manipulation attack.)

CREAM FINANCE hack (August 2021) ($18.8 million, reentrancy vulnerability allowed the exploiter for the second borrow.)

Siren protocol hack (September 2021) ($3.5 million, AMM pools were exploited through reentrancy attack.)


Type of Reentrancy:
[Details](https://inspexco.medium.com/cross-contract-reentrancy-attack-402d27a02a15)
1 - Single Function Reentrancy
2 - Cross-Function Reentrancy
3 - Cross-Contract Reentrancy


### Recommended Mitigation Steps

Use Openzeppelin or Solmate Re-Entrancy pattern
Here is a example of a re-entracy guard

```solidity
pragma solidity 0.8.17;

contract ReEntrancyGuard {
    bool internal locked;

    modifier noReentrant() {
        require(!locked, "No re-entrancy");
        locked = true;
        _;
        locked = false;
    }
}
```

### [L-13] Use UniswapV3’s `mulDiv` function for rigorous validation

Consider calculations that can cause overflow/underflow in uint256  rigorous validation and upgrade to the UniswapV3.muldiv()

https://github.com/Uniswap/v3-core/blob/412d9b236a1e75a98568d49b1aeb21e3a1430544/contracts/libraries/FullMath.sol#L14

https://xn--2-umb.com/21/muldiv/


```solidity
packages/v2-library/src/FullMath.sol:
  180      /// @return result The result.
  181:     function mulDiv(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) internal pure returns (uint256 result) {
  182:         (uint256 product0, uint256 product1) = mul512(multiplicand, multiplier);
  183: 
  184:         // Handle non-overflow cases, 256 by 256 division
  185:         if (product1 == 0) return result = product0.div(divisor, roundUp);
  186: 
  187:         // Make sure the result is less than 2**256.
  188:         // Also prevents divisor == 0
  189:         if (divisor <= product1) revert MulDivOverflow(multiplicand, multiplier, divisor);
  190: 
  191:         unchecked {
  192:             ///////////////////////////////////////////////
  193:             // 512 by 256 division.
  194:             ///////////////////////////////////////////////
  195: 
  196:             // Make division exact by subtracting the remainder from [product1 product0]
  197:             // Compute remainder using mulmod
  198:             uint256 remainder;
  199:             assembly {
  200:                 remainder := mulmod(multiplicand, multiplier, divisor)
  201:             }
  202:             // Subtract 256 bit number from 512 bit number
  203:             assembly {
  204:                 product1 := sub(product1, gt(remainder, product0))
  205:                 product0 := sub(product0, remainder)
  206:             }
  207: 
  208:             // Factor powers of two out of divisor
  209:             // Compute largest power of two divisor of divisor.
  210:             // Always >= 1.
  211:             uint256 twos;
  212:             twos = (0 - divisor) & divisor;
  213:             // Divide denominator by power of two
  214:             assembly {
  215:                 divisor := div(divisor, twos)
  216:             }
  217: 
  218:             // Divide [product1 product0] by the factors of two
  219:             assembly {
  220:                 product0 := div(product0, twos)
  221:             }
  222:             // Shift in bits from product1 into product0. For this we need
  223:             // to flip `twos` such that it is 2**256 / twos.
  224:             // If twos is zero, then it becomes one
  225:             assembly {
  226:                 twos := add(div(sub(0, twos), twos), 1)
  227:             }
  228:             product0 |= product1 * twos;
  229: 
  230:             // Invert divisor mod 2**256
  231:             // Now that divisor is an odd number, it has an inverse
  232:             // modulo 2**256 such that divisor * inv = 1 mod 2**256.
  233:             // Compute the inverse by starting with a seed that is correct
  234:             // correct for four bits. That is, divisor * inv = 1 mod 2**4
  235:             uint256 inv;
  236:             inv = (3 * divisor) ^ 2;
  237: 
  238:             // Now use Newton-Raphson iteration to improve the precision.
  239:             // Thanks to Hensel's lifting lemma, this also works in modular
  240:             // arithmetic, doubling the correct bits in each step.
  241:             inv *= 2 - divisor * inv; // inverse mod 2**8
  242:             inv *= 2 - divisor * inv; // inverse mod 2**16
  243:             inv *= 2 - divisor * inv; // inverse mod 2**32
  244:             inv *= 2 - divisor * inv; // inverse mod 2**64
  245:             inv *= 2 - divisor * inv; // inverse mod 2**128
  246:             inv *= 2 - divisor * inv; // inverse mod 2**256
  247: 
  248:             // Because the division is now exact we can divide by multiplying
  249:             // with the modular inverse of divisor. This will give us the
  250:             // correct result modulo 2**256. Since the precoditions guarantee
  251:             // that the outcome is less than 2**256, this is the final result.
  252:             // We don't need to compute the high bits of the result and product1
  253:             // is no longer required.
  254:             result = product0 * inv;
  255:         }
  256: 
  257:         if (roundUp && mulmod(multiplicand, multiplier, divisor) != 0) result++;
  258:     }


```


### [L-14] Storage Write Removal Bug On Conditional Early Termination

```solidity

packages/v2-library/src/BytesLib.sol:
  11  library BytesLib {
  12:     function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {
  13:         require(_length + 31 >= _length, "slice_overflow");
  14:         require(_bytes.length >= _start + _length, "slice_outOfBounds");
  15: 
  16:         bytes memory tempBytes;
  17: 
  18:         assembly {
  19:             switch iszero(_length)
  20:             case 0 {
  23:                 tempBytes := mload(0x40)
  33:                 let lengthmod := and(_length, 31)
  39:                 let mc := add(add(tempBytes, lengthmod), mul(0x20, iszero(lengthmod)))
  40:                 let end := add(mc, _length)
  41: 
  42:                 for {
  45:                     let cc := add(add(add(_bytes, lengthmod), mul(0x20, iszero(lengthmod))), _start)
  46:                 } lt(mc, end) {
  47:                     mc := add(mc, 0x20)
  48:                     cc := add(cc, 0x20)
  49:                 } {
  50:                     mstore(mc, mload(cc))
  51:                 }
  52: 
  53:                 mstore(tempBytes, _length)
 57:                 mstore(0x40, and(add(mc, 31), not(31)))
  58:             }
  59:             //if we want a zero-length slice let's just return a zero-length array
  60:             default {
  61:                 tempBytes := mload(0x40)
  62:                 //zero out the 32 bytes slice we are about to return
  63:                 //we need to do it because Solidity does not garbage collect
  64:                 mstore(tempBytes, 0)
  65: 
  66:                 mstore(0x40, add(tempBytes, 0x20))
  67:             }
  68:         }
  69: 
  70:         return tempBytes;
  71:     }
  72  }


```

### Proof Of Concept

https://twitter.com/solidity_lang/status/1567953562151579650?s=20&t=fXIo4hRjOiMXl2dqpD5Oyw
https://blog.soliditylang.org/2022/09/08/storage-write-removal-before-conditional-termination/


### Recommended Mitigation Steps
Upgrade  pragma to version 0.8.17



### [L-15] Critical technical faulty definition in the documents

```

README.md:

   ## Timeswap v2-Option
   Timeswap option is a close implementation of European option given 
   the caveats of writing smart contracts in Ethereum.
   Given a pair of tokens, defined as token0 and token1, 
   the option is fully collateralized with either one of the tokens. 
   When the option has locked token0, then it gives the holder the right 
   but not the obligation to swap token1 for token0 before the maturity of the option. 
   When the option has locked token1, then the holder can swap token0 for token1. 
   When the swap is exercised, the token that is deposited becomes the new locked 
   token of the option, giving the holder the right but not the obligation to swap 
   the tokens back before maturity. This design gives the holder the ability to swap 
   token0 and token1 back and forth at a given strike rate, for however
   many times before the expiry of the option.

```
https://github.com/code-423n4/2023-01-timeswap#timeswap-v2-option

In the explanations, it is stated that the project is close to the European Type Option, but with the explanation below, it is clearly understood that it is the American type option.

European Style Options: can be exercised only at expiration.
American Style Options: can be exercised at any time prior to expiration.

https://www.cmegroup.com/education/courses/introduction-to-options/understanding-the-difference-european-vs-american-style-options.html

```
This design gives the holder the ability to swap token0 and token1 back and forth at a given strike rate, 
for however many times before the expiry of the option.
```

### Recomendation Step:


Update the document

If the description is correct and it is a European Type Option, the architecture in the Codes is faulty and the problem can be classified as MEDIUM.


### [N-01] Remove Unused Codes

`OptionPair.checkNotZeroAddress()` and `PooPair.checkDoesNotExist` functions are unused code , I recommend removing unused codes from the codebase

```solidity
2 result

packages/v2-option/src/libraries/OptionPair.sol:
  20      /// @param optionPair The option pair address being inquired.
  21:     function checkNotZeroAddress(address optionPair) internal pure {
  22:         if (optionPair == address(0)) revert ZeroOptionAddress();
  23:     }

packages/v2-pool/src/libraries/PoolPair.sol:
  21      /// @param poolPair The address of the TimeswapV2Pool contract.
  22:     function checkDoesNotExist(address optionPair, address poolPair) internal pure {
  23:         if (poolPair != address(0)) revert PoolPairAlreadyExisted(optionPair, poolPair);
  24:     }
  25  }

```
### [N-02] Insufficient coverage

**Description:**
The test coverage rate of the project is ~80%. Testing all functions is best practice in terms of security criteria.
Due to its capacity, test coverage is expected to be 100%.


```js
1 result - 1 file

README.md:
  157:   - What is the overall line coverage percentage provided by your tests?: ~50 
```

### [N-03] NatSpec comments should be increased in contracts

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
NatSpec comments should be increased in contracts


### [N-04] `Function writing` that does not comply with the `Solidity Style Guide`

**Context:**
All Contracts

**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

 constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
within a grouping, place the view and pure functions last


### [N-05] Add a timelock to critical functions

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).
Consider adding a timelock to:

```solidity

1 result - 1 file

packages/v2-pool/src/base/OwnableTwoSteps.sol:
  22      /// @inheritdoc IOwnableTwoSteps
  23:     function setPendingOwner(address chosenPendingOwner) external override {
  24:         Ownership.checkIfOwner(owner);
  25: 
  26:         if (chosenPendingOwner == address(0)) Error.zeroAddress();
  27:         chosenPendingOwner.checkIfAlreadyOwner(owner);
  28: 
  29:         pendingOwner = chosenPendingOwner;
  30: 
  31:         emit SetOwner(pendingOwner);
  32:     }

```

### [N-06] Include return parameters in NatSpec comments

**Context:**
All Contracts

**Description:**
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

**Recommendation:**
Include return parameters in NatSpec comments

_Recommendation  Code Style: (from Uniswap3)_
```js
    /// @notice Adds liquidity for the given recipient/tickLower/tickUpper position
    /// @dev The caller of this method receives a callback in the form of IUniswapV3MintCallback#uniswapV3MintCallback
    /// in which they must pay any token0 or token1 owed for the liquidity. The amount of token0/token1 due depends
    /// on tickLower, tickUpper, the amount of liquidity, and the current price.
    /// @param recipient The address for which the liquidity will be created
    /// @param tickLower The lower tick of the position in which to add liquidity
    /// @param tickUpper The upper tick of the position in which to add liquidity
    /// @param amount The amount of liquidity to mint
    /// @param data Any data that should be passed through to the callback
    /// @return amount0 The amount of token0 that was paid to mint the given amount of liquidity. Matches the value in the callback
    /// @return amount1 The amount of token1 that was paid to mint the given amount of liquidity. Matches the value in the callback
    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);
```
### [N-07] Tokens accidentally sent to the contract cannot be recovered


It can't be recovered if the tokens accidentally arrive at the contract address, which has happened to many popular projects, so I recommend adding a recovery code to your critical contracts.

### Recommended Mitigation Steps

Add this code:

```solidity
 /**
  * @notice Sends ERC20 tokens trapped in contract to external address
  * @dev Onlyowner is allowed to make this function call
  * @param account is the receiving address
  * @param externalToken is the token being sent
  * @param amount is the quantity being sent
  * @return boolean value indicating whether the operation succeeded.
  *
 */
  function rescueERC20(address account, address externalToken, uint256 amount) public onlyOwner returns (bool) {
    IERC20(externalToken).transfer(account, amount);
    return true;
  }
}

```

### [N-08] Assembly Codes Specific – Should Have Comments

Since this is a low level language that is more difficult to parse by readers, include extensive documentation, comments on the rationale behind its use, clearly explaining what each assembly instruction does


This will make it easier for users to trust the code, for reviewers to validate the code, and for developers to build on or update the code.

Note that using Assembly removes several important security features of Solidity, which can make the code more insecure and more error-prone.

```solidity
10 results - 1 file

packages/v2-library/src/FullMath.sol:
   47:         assembly {
   48:             sum0 := add(addendA0, addendB0)
   63:         assembly {
   64:             difference0 := sub(minuend0, subtrahend0)
   78:         assembly {
   79:             let mm := mulmod(multiplicand, multiplier, not(0))
   92:         assembly {
   93:             quotient := add(div(sub(0, divisor), divisor), 1)
  103:         assembly {
  104:             result := mod(sub(0, value), value)
  199:             assembly {
  200:                 remainder := mulmod(multiplicand, multiplier, divisor)
  203:             assembly {
  204:                 product1 := sub(product1, gt(remainder, product0))
  214:             assembly {
  215:                 divisor := div(divisor, twos)
  219:             assembly {
  220:                 product0 := div(product0, twos)
  225:             assembly {
  226:                 twos := add(div(sub(0, twos), twos), 1)

```

### [N-09] Take advantage of Custom Error's return value property

An important feature of Custom Error is that values such as address, tokenID, msg.value can 
be written inside the `()` sign, this kind of approach provides a serious advantage in 
debugging and examining the revert details of dapps such as tenderly.


```solidity
38 results 

packages/v2-library/src/Error.sol:
  68:         revert ZeroInput();
  73:         revert ZeroOutput();
  78:         revert CannotBeZero();
  89:         revert RequireLiquidity();
  94:         revert ZeroAddress();

packages/v2-library/src/FullMath.sol:
   91:         if (divisor == 0) revert Math.DivideByZero();
  102:         if (value == 0) revert ModuloByZero();

packages/v2-library/src/Ownership.sol:
  14:     /// @dev revert when the caller is not the pending owner.

packages/v2-library/src/SafeCast.sol:
  19:         if (value > type(uint16).max) revert Uint16Overflow();
  28:         if (value > type(uint96).max) revert Uint96Overflow();
  37:         if (value > type(uint160).max) revert Uint160Overflow();

packages/v2-option/src/NoDelegateCall.sol:
  30:         if (address(this) != original) revert CannotBeDelegateCalled();

packages/v2-option/src/enums/Position.sol:
  23:         if (uint256(position) >= 3) revert InvalidPosition();

packages/v2-option/src/enums/Transaction.sol:
  37:         if (uint256(transaction) >= 2) revert InvalidTransaction();
  43:         if (uint256(transaction) >= 2) revert InvalidTransaction();
  49:         if (uint256(transaction) >= 2) revert InvalidTransaction();
  55:         if (uint256(transaction) >= 3) revert InvalidTransaction();

packages/v2-option/src/libraries/OptionFactory.sol:
  19:         if (optionFactory == address(0)) revert ZeroFactoryAddress();

packages/v2-option/src/libraries/OptionPair.sol:
  22:         if (optionPair == address(0)) revert ZeroOptionAddress();

packages/v2-pool/src/NoDelegateCall.sol:
  30:         if (address(this) != original) revert CannotBeDelegateCalled();

packages/v2-pool/src/TimeswapV2Pool.sol:
  289:         if (isQuote) revert Quote();
  355:         if (isQuote) revert Quote();
  407:         if (isQuote) revert Quote();
  457:         if (isQuote) revert Quote();

packages/v2-pool/src/enums/Transaction.sol:
  48:         revert InvalidTransaction();
  53:         if (uint256(transaction) >= 4) revert InvalidTransaction();
  58:         if (uint256(transaction) >= 4) revert InvalidTransaction();
  63:         if (uint256(transaction) >= 4) revert InvalidTransaction();
  68:         if (uint256(transaction) >= 4) revert InvalidTransaction();
  73:         if (uint256(transaction) >= 2) revert InvalidTransaction();

packages/v2-pool/src/libraries/ConstantProduct.sol:
  298:             if (product.div(longAmount, false) != rate || product >= numerator) revert NotEnoughLiquidityToBorrow();
  320:         else revert NotEnoughLiquidityToLend();
  421:             if (a11 != 0 || a01.unsafeAdd(a10) < a01) revert CalculationOverload();

packages/v2-pool/src/libraries/FeeCalculation.sol:
  16:         revert FeeOverflow();

packages/v2-pool/src/libraries/PoolFactory.sol:
  19:         if (poolFactory == address(0)) revert ZeroFactoryAddress();

packages/v2-pool/src/libraries/PoolPair.sol:
  16:         if (poolPair == address(0)) revert ZeroSwapAddress();

packages/v2-pool/src/libraries/ReentrancyGuard.sol:
  24:         if (reentrancyGuard == NOT_INTERACTED) revert NotInteracted();
  25:         if (reentrancyGuard == ENTERED) revert NoReentrantCall();

```

### [N-10] Repeated validation logic can be converted into a function modifier

If a query or logic is repeated over many lines, using a modifier improves the readability and reusability of the code


```solidity

37 results - 14 files

packages/v2-option/src/TimeswapV2Option.sol:
  98          if (!hasInteracted[strike][maturity]) Error.inactiveOptionChoice(strike, maturity);
  99:         if (to == address(0)) Error.zeroAddress();

packages/v2-option/src/TimeswapV2OptionFactory.sol:
  43      function create(address token0, address token1) external override returns (address optionPair) {
  44:         if (token0 == address(0)) Error.zeroAddress();
  45:         if (token1 == address(0)) Error.zeroAddress();

packages/v2-option/src/libraries/OptionFactory.sol:
  18      function checkNotZeroFactory(address optionFactory) internal pure {
  19:         if (optionFactory == address(0)) revert ZeroFactoryAddress();
  39:         if (optionPair == address(0)) Error.zeroAddress();

packages/v2-option/src/libraries/OptionPair.sol:
  21      function checkNotZeroAddress(address optionPair) internal pure {
  22:         if (optionPair == address(0)) revert ZeroOptionAddress();

packages/v2-option/src/structs/Param.sol:
  107:         if (param.shortTo == address(0)) Error.zeroAddress();
  108:         if (param.long0To == address(0)) Error.zeroAddress();
  109:         if (param.long1To == address(0)) Error.zeroAddress();

  121:         if (param.token0To == address(0)) Error.zeroAddress();
  122:         if (param.token1To == address(0)) Error.zeroAddress();

  134:         if (param.tokenTo == address(0)) Error.zeroAddress();
  135:         if (param.longTo == address(0)) Error.zeroAddress();

  147:         if (param.token0To == address(0)) Error.zeroAddress();
  148:         if (param.token1To == address(0)) Error.zeroAddress();

packages/v2-pool/src/TimeswapV2Pool.sol:
  156:         if (to == address(0)) Error.zeroAddress();

  165      function transferFees(uint256 strike, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external override {
  166:         if (to == address(0)) Error.zeroAddress();

packages/v2-pool/src/TimeswapV2PoolFactory.sol:
  59      function create(address optionPair) external override returns (address pair) {
  60:         if (optionPair == address(0)) Error.zeroAddress();

packages/v2-pool/src/base/OwnableTwoSteps.sol:
  26:         if (chosenPendingOwner == address(0)) Error.zeroAddress();

packages/v2-pool/src/libraries/PoolFactory.sol:
  18      function checkNotZeroFactory(address poolFactory) internal pure {
  19:         if (poolFactory == address(0)) revert ZeroFactoryAddress();
  48:         if (poolPair == address(0)) Error.zeroAddress();

packages/v2-pool/src/libraries/PoolPair.sol:
  15      function checkNotZeroAddress(address poolPair) internal pure {
  16:         if (poolPair == address(0)) revert ZeroSwapAddress();
  17      }

packages/v2-pool/src/structs/Param.sol:
  133      function check(TimeswapV2PoolCollectParam memory param) internal pure {
  134:         if (param.long0To == address(0) || param.long1To == address(0) || param.shortTo == address(0)) Error.zeroAddress();
  135          if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);

  144          if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
  145:         if (param.to == address(0)) Error.zeroAddress();
  146          TransactionLibrary.check(param.transaction);

  155          if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
  156:         if (param.long0To == address(0) || param.long1To == address(0) || param.shortTo == address(0)) Error.zeroAddress();
  157  

  167          if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
  168:         if (param.to == address(0)) Error.zeroAddress();
  169          TransactionLibrary.check(param.transaction);

  178          if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
  179:         if (param.long0To == address(0) || param.long1To == address(0)) Error.zeroAddress();
  180  

  190          if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
  191:         if (param.to == address(0)) Error.zeroAddress();
  192          TransactionLibrary.check(param.transaction);

packages/v2-token/src/TimeswapV2LiquidityToken.sol:
  75      function transferFeesFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external override {
  76:         if (from == address(0)) Error.zeroAddress();
  77:         if (to == address(0)) Error.zeroAddress();
  78  

packages/v2-token/src/base/ERC1155Enumerable.sol:
  58      function _addTokenEnumeration(address from, address to, uint256 id, uint256 amount) internal {
  59:         if (from == address(0)) {
  60              if (_idTotalSupply[id] == 0 && _additionalConditionAddTokenToAllTokensEnumeration(id)) _addTokenToAllTokensEnumeration(id);

  92      function _removeTokenEnumeration(address from, address to, uint256 id, uint256 amount) internal {
  93:         if (to == address(0)) {
  94              if (_idTotalSupply[id] == 0 && _additionalConditionRemoveTokenFromAllTokensEnumeration(id)) _removeTokenFromAllTokensEnumeration(id);

packages/v2-token/src/structs/Param.sol:
  118      function check(TimeswapV2TokenMintParam memory param) internal pure {
  119:         if (param.long0To == address(0) || param.long1To == address(0) || param.shortTo == address(0)) Error.zeroAddress();


  125      function check(TimeswapV2TokenBurnParam memory param) internal pure {
  126:         if (param.long0To == address(0) || param.long1To == address(0) || param.shortTo == address(0)) Error.zeroAddress();

  132      function check(TimeswapV2LiquidityTokenMintParam memory param) internal pure {
  133:         if (param.to == address(0)) Error.zeroAddress();

  139      function check(TimeswapV2LiquidityTokenBurnParam memory param) internal pure {
  140:         if (param.to == address(0)) Error.zeroAddress();

  146      function check(TimeswapV2LiquidityTokenCollectParam memory param) internal pure {
  147:         if (param.to == address(0)) Error.zeroAddress();

```


### [N-11] For modern and more readable code; update import usages

**Context:**

```solidity

packages/v2-token/src/interfaces/IERC1155Enumerable.sol:

6: import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
```


**Description:**
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct `polluted the source code` with an unnecessary object we were not using because we did not need it. 
This was breaking the rule of modularity and modular programming: `only import what you need` Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
`import {contract1 , contract2} from "filename.sol";`

A good example from the ArtGobblers project;
```js
import {Owned} from "solmate/auth/Owned.sol";
import {ERC721} from "solmate/tokens/ERC721.sol";
import {LibString} from "solmate/utils/LibString.sol";
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```


### [N-12] Use a more recent version of Solidity

**Context:**
All contracts

**Description:**
For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md


**Recommendation:**
Old version of Solidity is used `(0.8.8)`, newer version can be used `(0.8.17)` 

### [N-13] For functions, follow Solidity standard naming conventions (internal function style rule)

**Context:**
```solidity

139 results - 37 files

packages/v2-library/src/BytesLib.sol:
  12:     function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {

packages/v2-library/src/CatchError.sol:
  12:     function catchError(bytes memory reason, bytes4 selector) internal pure returns (bytes memory) {

packages/v2-library/src/Error.sol:
   67:     function zeroInput() internal pure {
   72:     function zeroOutput() internal pure {
   77:     function cannotBeZero() internal pure {
   83:     function alreadyHaveLiquidity(uint160 liquidity) internal pure {
   88:     function requireLiquidity() internal pure {
   93:     function zeroAddress() internal pure {
   99:     function incorrectMaturity(uint256 maturity) internal pure {
  106:     function alreadyMatured(uint256 maturity, uint96 blockTimestamp) internal pure {
  113:     function stillActive(uint256 maturity, uint96 blockTimestamp) internal pure {
  119:     function deadlineReached(uint256 deadline) internal pure {
  125:     function inactiveOptionChoice(uint256 strike, uint256 maturity) internal pure {
  132:     function inactivePoolChoice(uint256 strike, uint256 maturity) internal pure {
  139:     function zeroSqrtInterestRate(uint256 strike, uint256 maturity) internal pure {
  144:     function inactiveLiquidityTokenChoice() internal pure {
  151:     function checkEnough(uint256 balance, uint256 balanceTarget) internal pure {

packages/v2-library/src/FullMath.sol:
   46:     function add512(uint256 addendA0, uint256 addendA1, uint256 addendB0, uint256 addendB1) internal pure returns (uint256 sum0, uint256 sum1) {
   62:     function sub512(uint256 minuend0, uint256 minuend1, uint256 subtrahend0, uint256 subtrahend1) internal pure returns (uint256 difference0, uint256 difference1) {
   77:     function mul512(uint256 multiplicand, uint256 multiplier) internal pure returns (uint256 product0, uint256 product1) {
  138:     function div512To256(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
  158:     function div512(uint256 dividend0, uint256 dividend1, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient0, uint256 quotient1) {
  181:     function mulDiv(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) internal pure returns (uint256 result) {
  265:     function sqrt512(uint256 value0, uint256 value1, bool roundUp) internal pure returns (uint256 result) {

packages/v2-library/src/Math.sol:
  14:     function unsafeAdd(uint256 addend1, uint256 addend2) internal pure returns (uint256 sum) {
  25:     function unsafeSub(uint256 minuend, uint256 subtrahend) internal pure returns (uint256 difference) {
  36:     function unsafeMul(uint256 multiplicand, uint256 multiplier) internal pure returns (uint256 product) {
  48:     function div(uint256 dividend, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
  59:     function shr(uint256 dividend, uint8 divisorBit, bool roundUp) internal pure returns (uint256 quotient) {
  69:     function sqrt(uint256 value, bool roundUp) internal pure returns (uint256 result) {
  88:     function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {

packages/v2-library/src/Ownership.sol:
  22:     function checkIfOwner(address owner) internal view {
  30:     function checkIfAlreadyOwner(address chosenPendingOwner, address owner) internal pure {
  38:     function checkIfPendingOwner(address caller, address pendingOwner) internal pure {

packages/v2-library/src/SafeCast.sol:
  18:     function toUint16(uint256 value) internal pure returns (uint16 result) {
  27:     function toUint96(uint256 value) internal pure returns (uint96 result) {
  36:     function toUint160(uint256 value) internal pure returns (uint160 result) {

packages/v2-library/src/StrikeConversion.sol:
  16:     function convert(uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) internal pure returns (uint256) {
  26:     function turn(uint256 amount, uint256 strike, bool toOne, bool roundUp) internal pure returns (uint256) {
  35:     function combine(uint256 amount0, uint256 amount1, uint256 strike, bool roundUp) internal pure returns (uint256) {
  46:     function dif(uint256 base, uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) internal pure returns (uint256) {

packages/v2-option/src/TimeswapV2Option.sol:
  70:     function blockTimestamp() internal view virtual returns (uint96) {

packages/v2-option/src/TimeswapV2OptionDeployer.sol:
  32:     function deploy(address optionFactory, address token0, address token1) internal returns (address optionPair) {

packages/v2-option/src/enums/Position.sol:
  22:     function check(TimeswapV2OptionPosition position) internal pure {

packages/v2-option/src/enums/Transaction.sol:
  36:     function check(TimeswapV2OptionMint transaction) internal pure {
  42:     function check(TimeswapV2OptionBurn transaction) internal pure {
  48:     function check(TimeswapV2OptionSwap transaction) internal pure {
  54:     function check(TimeswapV2OptionCollect transaction) internal pure {

packages/v2-option/src/libraries/OptionFactory.sol:
  18:     function checkNotZeroFactory(address optionFactory) internal pure {
  27:     function get(address optionFactory, address token0, address token1) internal view returns (address optionPair) {
  37:     function getWithCheck(address optionFactory, address token0, address token1) internal view returns (address optionPair) {

packages/v2-option/src/libraries/OptionPair.sol:
  21:     function checkNotZeroAddress(address optionPair) internal pure {
  30:     function checkCorrectFormat(address token0, address token1) internal pure {
  39:     function checkDoesNotExist(address token0, address token1, address optionPair) internal pure {

packages/v2-option/src/libraries/Proportion.sol:
  12:     function proportion(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) internal pure returns (uint256) {

packages/v2-option/src/structs/Option.sol:
   28: /// @dev internal library handling important business logic of the option.
   38:     function totalPosition(Option storage option, uint256 strike, TimeswapV2OptionPosition position) internal view returns (uint256 balance) {
   49:     function positionOf(Option storage option, address owner, TimeswapV2OptionPosition position) internal view returns (uint256 balance) {
   60:     function transferPosition(Option storage option, address to, TimeswapV2OptionPosition position, uint256 amount) internal {
   94:     ) internal returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount) {
  133:     ) internal returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount) {
  163:     ) internal returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount) {
  191:     function collect(Option storage option, uint256 strike, TimeswapV2OptionCollect transaction, uint256 amount) internal returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount) {

packages/v2-option/src/structs/Param.sol:
  103:     function check(TimeswapV2OptionMintParam memory param, uint96 blockTimestamp) internal pure {
  117:     function check(TimeswapV2OptionBurnParam memory param, uint96 blockTimestamp) internal pure {
  130:     function check(TimeswapV2OptionSwapParam memory param, uint96 blockTimestamp) internal pure {
  143:     function check(TimeswapV2OptionCollectParam memory param, uint96 blockTimestamp) internal pure {

packages/v2-option/src/structs/Process.sol:
  33:     function updateProcess(Process[] storage processing, uint256 token0Amount, uint256 token1Amount, bool isAddToken0, bool isAddToken1) internal {

packages/v2-pool/src/TimeswapV2Pool.sol:
  82:     function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {

packages/v2-pool/src/TimeswapV2PoolDeployer.sol:
  25:     function deploy(address poolFactory, address optionPair, uint256 transactionFee, uint256 protocolFee) internal returns (address poolPair) {

packages/v2-pool/src/enums/Transaction.sol:
  47:     function invalidTransaction() internal pure {
  52:     function check(TimeswapV2PoolMint transaction) internal pure {
  57:     function check(TimeswapV2PoolBurn transaction) internal pure {
  62:     function check(TimeswapV2PoolDeleverage transaction) internal pure {
  67:     function check(TimeswapV2PoolLeverage transaction) internal pure {
  72:     function check(TimeswapV2PoolRebalance transaction) internal pure {

packages/v2-pool/src/libraries/ConstantProduct.sol:
   29:     function getLong(uint160 liquidity, uint160 rate, bool roundUp) internal pure returns (uint256) {
   38:     function getShort(uint160 liquidity, uint160 rate, uint96 duration, bool roundUp) internal pure returns (uint256) {
   51:     function calculateGivenLiquidityDelta(uint160 rate, uint160 deltaLiquidity, uint96 duration, bool isAdd) internal pure returns (uint256 longAmount, uint256 shortAmount) {
   66:     function calculateGivenLiquidityLong(uint160 rate, uint256 longAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 shortAmount) {
   81:     function calculateGivenLiquidityShort(uint160 rate, uint256 shortAmount, uint96 duration, bool isAdd) internal pure returns (uint160 liquidityAmount, uint256 longAmount) {
  103:     ) internal pure returns (uint160 liquidityAmount, uint256 longAmount, uint256 shortAmount) {
  141:     ) internal pure returns (uint160 newRate, uint256 longAmount, uint256 shortAmount, uint256 fees) {
  171:     ) internal pure returns (uint160 newRate, uint256 shortAmount, uint256 fees) {
  202:     ) internal pure returns (uint160 newRate, uint256 longAmount, uint256 fees) {
  237:     ) internal pure returns (uint160 newRate, uint256 longAmount, uint256 shortAmount, uint256 fees) {

packages/v2-pool/src/libraries/ConstantSum.sol:
  19:     function calculateGivenLongIn(uint256 strike, uint256 longAmountIn, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountOut, uint256 longFees) {
  30:     function calculateGivenLongOut(uint256 strike, uint256 longAmountOut, uint256 transactionFee, bool isLong0ToLong1) internal pure returns (uint256 longAmountIn, uint256 longFees) {
  39:     function calculateGivenLongOutAlreadyAdjustFees(uint256 strike, uint256 longAmountOut, bool isLong0ToLong1) internal pure returns (uint256 longAmountIn) {

packages/v2-pool/src/libraries/Duration.sol:
  11:     function init(uint256 duration) internal pure returns (uint96) {

packages/v2-pool/src/libraries/DurationCalculation.sol:
  18:     function unsafeDurationFromLastTimestampToNow(uint96 lastTimestamp, uint96 blockTimestamp) internal pure returns (uint96 duration) {
  26:     function unsafeDurationFromNowToMaturity(uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {
  34:     function unsafeDurationFromLastTimestampToMaturity(uint96 lastTimestamp, uint256 maturity) internal pure returns (uint96 duration) {
  43:     function unsafeDurationFromLastTimestampToNowOrMaturity(uint96 lastTimestamp, uint256 maturity, uint96 blockTimestamp) internal pure returns (uint96 duration) {

packages/v2-pool/src/libraries/DurationWeight.sol:
  15:     function update(uint160 liquidity, uint256 shortFeeGrowth, uint256 shortAmount) internal pure returns (uint256 newShortFeeGrowth) {

packages/v2-pool/src/libraries/Fee.sol:
  10:     function check(uint256 fee) internal pure {

packages/v2-pool/src/libraries/FeeCalculation.sol:
  27:     function update(uint160 liquidity, uint256 feeGrowth, uint256 protocolFees, uint256 fees, uint256 protocolFee) internal pure returns (uint256 newFeeGrowth, uint256 newProtocolFees) {
  40:     function getFees(uint160 liquidity, uint256 lastFeeGrowth, uint256 globalFeeGrowth) internal pure returns (uint256) {
  47:     function addFees(uint256 amount, uint256 fee) internal pure returns (uint256) {
  54:     function removeFees(uint256 amount, uint256 fee) internal pure returns (uint256) {
  61:     function getFeesRemoval(uint256 amount, uint256 fee) internal pure returns (uint256) {
  68:     function getFeesAdditional(uint256 amount, uint256 fee) internal pure returns (uint256) {
  75:     function getFeeGrowth(uint256 feeAmount, uint160 liquidity) internal pure returns (uint256) {

packages/v2-pool/src/libraries/PoolFactory.sol:
  18:     function checkNotZeroFactory(address poolFactory) internal pure {
  29:     function get(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {
  43:     function getWithCheck(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {

packages/v2-pool/src/libraries/PoolPair.sol:
  15:     function checkNotZeroAddress(address poolPair) internal pure {
  22:     function checkDoesNotExist(address optionPair, address poolPair) internal pure {

packages/v2-pool/src/libraries/ReentrancyGuard.sol:
  12:     uint96 internal constant NOT_INTERACTED = 0;
  15:     uint96 internal constant NOT_ENTERED = 1;
  18:     uint96 internal constant ENTERED = 2;
  23:     function check(uint96 reentrancyGuard) internal pure {

packages/v2-pool/src/structs/LiquidityPosition.sol:
  38:     ) internal pure returns (uint256 long0Fee, uint256 long1Fee, uint256 shortFee) {
  51:     function update(LiquidityPosition storage liquidityPosition, uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) internal {
  65:     function mint(LiquidityPosition storage liquidityPosition, uint160 liquidityAmount) internal {
  69:     function mintFees(LiquidityPosition storage liquidityPosition, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) internal {
  75:     function burn(LiquidityPosition storage liquidityPosition, uint160 liquidityAmount) internal {
  79:     function burnFees(LiquidityPosition storage liquidityPosition, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) internal {
  90:     ) internal returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

packages/v2-pool/src/structs/Param.sol:
  133:     function check(TimeswapV2PoolCollectParam memory param) internal pure {
  142:     function check(TimeswapV2PoolMintParam memory param, uint96 blockTimestamp) internal pure {
  153:     function check(TimeswapV2PoolBurnParam memory param, uint96 blockTimestamp) internal pure {
  165:     function check(TimeswapV2PoolDeleverageParam memory param, uint96 blockTimestamp) internal pure {
  176:     function check(TimeswapV2PoolLeverageParam memory param, uint96 blockTimestamp) internal pure {
  188:     function check(TimeswapV2PoolRebalanceParam memory param, uint96 blockTimestamp) internal pure {

packages/v2-token/src/TimeswapV2Token.sol:
  51:     /// @dev internal function to start the reentrancy guard
  59:     /// @dev internal function to end the reentrancy guard

packages/v2-token/src/structs/FeesPosition.sol:
  23:     ) internal pure returns (uint256 long0Fee, uint256 long1Fee, uint256 shortFee) {
  29:     function update(FeesPosition storage feesPosition, uint160 liquidity, uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) internal {
  46:     ) internal view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
  52:     function mint(FeesPosition storage feesPosition, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) internal {
  58:     function burn(FeesPosition storage feesPosition, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) internal {

packages/v2-token/src/structs/Param.sol:
  118:     function check(TimeswapV2TokenMintParam memory param) internal pure {
  125:     function check(TimeswapV2TokenBurnParam memory param) internal pure {
  132:     function check(TimeswapV2LiquidityTokenMintParam memory param) internal pure {
  139:     function check(TimeswapV2LiquidityTokenBurnParam memory param) internal pure {
  146:     function check(TimeswapV2LiquidityTokenCollectParam memory param) internal pure {

packages/v2-token/src/structs/Position.sol:
  34:     function toKey(TimeswapV2TokenPosition memory timeswapV2TokenPosition) internal pure returns (bytes32) {
  39:     function toKey(TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition) internal pure returns (bytes32) {

```


**Description:**
The above codes don't follow Solidity's standard naming convention,

internal and private functions : the mixedCase format starting with an underscore (_mixedCase starting with an underscore)

### [N-14] Floating pragma

**Description:**
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
https://swcregistry.io/docs/SWC-103

Floating Pragma List: 
```solidity

8 results - 8 files

packages/v2-token/src/TimeswapV2LiquidityToken.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.8;
  3  

packages/v2-token/src/TimeswapV2Token.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.8;
  3  

packages/v2-token/src/base/ERC1155Enumerable.sol:
  3  
  4: pragma solidity ^0.8.0;
  5  

packages/v2-token/src/interfaces/IERC1155Enumerable.sol:
  3  
  4: pragma solidity ^0.8.0;
  5  

packages/v2-token/src/structs/CallbackParam.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.8;
  3  

packages/v2-token/src/structs/FeesPosition.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.8;
  3  

packages/v2-token/src/structs/Param.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.8;
  3  

packages/v2-token/src/structs/Position.sol:
  1  // SPDX-License-Identifier: MIT
  2: pragma solidity ^0.8.8;
  3 

```


**Recommendation:**
Lock the pragma version and also consider known bugs (https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.

### [N-15] Project Upgrade and Stop Scenario should be

At the start of the project, the system may need to be stopped or upgraded, I suggest you have a script beforehand and add it to the documentation.
This can also be called an " EMERGENCY STOP (CIRCUIT BREAKER) PATTERN ".

https://github.com/maxwoe/solidity_patterns/blob/master/security/EmergencyStop.sol

### [N-16] Use descriptive names for Contracts and Libraries


This codebase will be difficult to navigate, as there are no descriptive naming conventions that specify which files should contain meaningful logic.

Prefixes should be added like this by filing:

Interface I_
absctract contracts Abs_
Libraries Lib_

We recommend that you implement this or a similar agreement.

### [N-17] Use SMTChecker

The *highest* tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification.

https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19

### [N-18] Remove the codes used in the test area in the actual codes


```solidity
1 result - 1 file

packages/v2-token/src/TimeswapV2Token.sol:
  4  import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
  5: import "forge-std/console.sol";
```

### [N-19]  Add NatSpec Mapping comment

**Description:**
Add NatSpec comments describing mapping keys and values


**Context:**

```solidity
29 results - 9 files

packages/v2-option/src/TimeswapV2Option.sol:
  48:     mapping(uint256 => mapping(uint256 => Option)) private options;
  49      /// @dev Always start and end as an empty array for every transaction.

  52  
  53:     mapping(uint256 => mapping(uint256 => bool)) private hasInteracted;
  54      StrikeAndMaturity[] private listOfOptions;

packages/v2-option/src/TimeswapV2OptionFactory.sol:
  21  
  22:     mapping(address => mapping(address => address)) private optionPairs;
  23  

packages/v2-option/src/structs/Option.sol:
  22      uint256 totalLong1;
  23:     mapping(address => uint256) long0;
  24:     mapping(address => uint256) long1;
  25:     mapping(address => uint256) short;
  26  }

packages/v2-pool/src/TimeswapV2Pool.sol:
  51  
  52:     mapping(uint256 => mapping(uint256 => uint96)) private reentrancyGuards;
  53:     mapping(uint256 => mapping(uint256 => Pool)) private pools;
  54  

packages/v2-pool/src/TimeswapV2PoolFactory.sol:
  30  
  31:     mapping(address => address) private pairs;
  32  

packages/v2-pool/src/structs/Pool.sol:
  52      uint256 shortProtocolFees;
  53:     mapping(address => LiquidityPosition) liquidityPositions;
  54  }

packages/v2-token/src/TimeswapV2LiquidityToken.sol:
  40  
  41:     mapping(bytes32 => uint96) private reentrancyGuards;
  42  
  43:     mapping(uint256 => TimeswapV2LiquidityTokenPosition) private _timeswapV2LiquidityTokenPositions;
  44  
  45:     mapping(bytes32 => uint256) private _timeswapV2LiquidityTokenPositionIds;
  46  
  47:     mapping(uint256 => mapping(address => FeesPosition)) private _feesPositions;
  48  

packages/v2-token/src/TimeswapV2Token.sol:
  35  
  36:     mapping(bytes32 => uint96) private reentrancyGuards;
  37  
  38:     mapping(uint256 => TimeswapV2TokenPosition) private _timeswapV2TokenPositions;
  39:     mapping(bytes32 => uint256) private _timeswapV2TokenPositionIds;
  40  

packages/v2-token/src/base/ERC1155Enumerable.sol:
  15:     mapping(address => mapping(uint256 => uint256)) private _ownedTokens; // An index of all tokens
  16  
  17:     mapping(address => uint256) private _currentIndex; // the current index for an address
  18  
  20:     mapping(uint256 => uint256) private _ownedTokensIndex;
  21  
  22:     mapping(uint256 => uint256) private _idTotalSupply;
  23  
  28:     mapping(uint256 => uint256) private _allTokensIndex;


```
**Recommendation:**

```solidity

 /// @dev    address(user) -> address(ERC0 Contract Address) -> uint256(allowance amount from user)
    mapping(address => mapping(address => uint256)) public allowance;

```


## [N-20] There is no need to cast a variable that is already an address, such as address(x)

```diff
packages/v2-option/src/TimeswapV2OptionDeployer.sol:
  34  
- 35:         optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
+ 35:         optionPair = new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}();

```
**Description:**
There is no need to cast a variable that is already an address, such as address(....) optionPair also address.

**Recommendation:**
Use directly variable.

### [N-21] Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases
Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that
length
Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

There are many examples, some of which are listed below;

[TimeswapV2Option.sol#L26-L27](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L26-L27)
[TimeswapV2Option.sol#L85](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L85)
[TimeswapV2Option.sol#L90](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L90)
[TimeswapV2Option.sol#L97](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L97)
[TimeswapV2Option.sol#L111](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L111)
[TimeswapV2Option.sol#L153](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L153)
[TimeswapV2Option.sol#L166](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L166)
[TimeswapV2Option.sol#L215](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L215)
[TimeswapV2Option.sol#L216](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L216)


### [N-22] Assembly Codes Specific – Should Have Comments

Since this is a low level language that is more difficult to parse by readers, include extensive documentation, comments on the rationale behind its use, clearly explaining what each assembly instruction does


This will make it easier for users to trust the code, for reviewers to validate the code, and for developers to build on or update the code.

Note that using Aseembly removes several important security features of Solidity, which can make the code more insecure and more error-prone.


```solidity
packages/v2-library/src/CatchError.sol:
  11      /// @param selector The given conditional selector.
  12:     function catchError(bytes memory reason, bytes4 selector) internal pure returns (bytes memory) {
  13:         uint256 length = reason.length;
  14: 
  15:         if ((length - 4) % 32 == 0 && bytes4(reason) == selector) return BytesLib.slice(reason, 4, length - 4);
  16: 
  17:         assembly {
  18:             revert(reason, length)
  19:         }


packages/v2-library/src/FullMath.sol:
  76      /// @return product1 The most significant part of product.
  77:     function mul512(uint256 multiplicand, uint256 multiplier) internal pure returns (uint256 product0, uint256 product1) {
  78:         assembly {
  79:             let mm := mulmod(multiplicand, multiplier, not(0))
  80:             product0 := mul(multiplicand, multiplier)
  81:             product1 := sub(sub(mm, product0), lt(mm, product0))
  82:         }
  83:     }

```

### [N-23] Move owner checks to a modifier

It's better to use a modifier for simple owner checks for an easier inspection of functions. In a function it can be forgotten and with a modifier it's easier to see how the function is protected.

**The part where owner is defined;**

```solidity

packages/v2-library/src/Ownership.sol:
  22:     function checkIfOwner(address owner) internal view {
  23          if (msg.sender != owner) revert NotTheOwner(msg.sender, owner);

```

 **2 result ;**
```solidity
packages/v2-pool/src/TimeswapV2Pool.sol:
  189:         ITimeswapV2PoolFactory(poolFactory).owner().checkIfOwner();
  190  

packages/v2-pool/src/base/OwnableTwoSteps.sol:
  23      function setPendingOwner(address chosenPendingOwner) external override {
  24:         Ownership.checkIfOwner(owner);

```
