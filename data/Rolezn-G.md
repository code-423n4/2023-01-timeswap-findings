## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Contexts|Estimated Gas Saved|
|-|:-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | `abi.encode()` is less efficient than `abi.encodepacked()` | 9 | 900 |
| [GAS&#x2011;2](#GAS&#x2011;2) | Setting the `constructor` to `payable` | 10 | 130 |
| [GAS&#x2011;3](#GAS&#x2011;3) | Using `delete` statement can save gas | 11 | - |
| [GAS&#x2011;4](#GAS&#x2011;4) | Use hardcoded address instead `address(this)` | 35 | - |
| [GAS&#x2011;5](#GAS&#x2011;5) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 3 | - |
| [GAS&#x2011;6](#GAS&#x2011;6) | Optimize names to save gas | 14 | 308 |
| [GAS&#x2011;7](#GAS&#x2011;7) | `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables | 73 | - |
| [GAS&#x2011;8](#GAS&#x2011;8) | Public Functions To External | 1 | - |
| [GAS&#x2011;9](#GAS&#x2011;9) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 85 | - |
| [GAS&#x2011;10](#GAS&#x2011;10) | Use solidity version 0.8.17 to gain some gas boost | 68 | 5984 |
| [GAS&#x2011;11](#GAS&#x2011;11) | Using `storage` instead of `memory` saves gas | 5 | 1750 |

Total: 314 contexts over 11 issues

## Gas Optimizations

### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> `abi.encode()` is less efficient than `abi.encodepacked()`

See for more information: https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison 

#### <ins>Proof Of Concept</ins>


```solidity
35: optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol#L35

```solidity
28: poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol#L28

```solidity
46: bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L46

```solidity
53: bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L53

```solidity
61: bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L61

```solidity
35: return keccak256(abi.encode(timeswapV2TokenPosition));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L35

```solidity
40: return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L40

```solidity
35: return keccak256(abi.encode(timeswapV2TokenPosition));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L35

```solidity
40: return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L40






### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> Setting the `constructor` to `payable`

Saves ~13 gas per instance

#### <ins>Proof Of Concept</ins>

```solidity
19: constructor()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/NoDelegateCall.sol#L19

```solidity
19: constructor()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/NoDelegateCall.sol#L19

```solidity
65: constructor() NoDelegateCall()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L65

```solidity
19: constructor()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/NoDelegateCall.sol#L19

```solidity
19: constructor()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/NoDelegateCall.sol#L19

```solidity
77: constructor() NoDelegateCall()
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L77

```solidity
37: constructor(address chosenOwner, uint256 chosenTransactionFee, uint256 chosenProtocolFee) OwnableTwoSteps(chosenOwner)
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37

```solidity
18: constructor(address chosenOwner)
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L18

```solidity
36: constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address")
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36

```solidity
41: constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address")
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L41










### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> Using `delete` statement can save gas

#### <ins>Proof Of Concept</ins>

```solidity
166: quotient0 = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/FullMath.sol#L166

```solidity
93: liquidityPosition.long0Fees = 0;
101: liquidityPosition.long1Fees = 0;
109: liquidityPosition.shortFees = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L93

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L101

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L109



```solidity
228: pool.long0ProtocolFees = 0;
236: pool.long1ProtocolFees = 0;
244: pool.shortProtocolFees = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L228

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L236

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L244



```solidity
633: pool.long0Balance = 0;
646: pool.long1Balance = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L633

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L646



```solidity
685: pool.long1Balance = 0;
706: pool.long0Balance = 0;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L685

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L706







### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> Use hardcode address instead `address(this)`

Instead of using `address(this)`, it is more gas-efficient to pre-calculate and use the hardcoded `address`. Foundry's script.sol and solmate's `LibRlp.sol` contracts can help achieve this.

References: 
https://book.getfoundry.sh/reference/forge-std/compute-create-address 

https://twitter.com/transmissions11/status/1518507047943245824

#### <ins>Proof Of Concept</ins>

```solidity
30: if (address(this) != original) revert CannotBeDelegateCalled();

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/NoDelegateCall.sol#L30

```solidity
30: if (address(this) != original) revert CannotBeDelegateCalled();

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/NoDelegateCall.sol#L30

```solidity
128: IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
129: IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount
145: if (token0AndLong0Amount != 0) Error.checkEnough(IERC20(token0).balanceOf(address(this)), currentProcess.balance0Target);
148: if (token1AndLong1Amount != 0) Error.checkEnough(IERC20(token1).balanceOf(address(this)), currentProcess.balance1Target);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L128

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L129

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L145

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L148



```solidity
215: param.isLong0ToLong1 ? IERC20(token0).balanceOf(address(this)) - token0AndLong0Amount : IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
216: param.isLong0ToLong1 ? IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount : IERC20(token1).balanceOf(address(this)) - token1AndLong1Amount
235: Error.checkEnough(IERC20(param.isLong0ToLong1 ? token1 : token0).balanceOf(address(this)), param.isLong0ToLong1 ? currentProcess.balance1Target : currentProcess.balance0Target);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L215

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L216

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L235



```solidity
51: optionPair = deploy(address(this), token0, token1);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L51

```solidity
30: if (address(this) != original) revert CannotBeDelegateCalled();

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/NoDelegateCall.sol#L30

```solidity
30: if (address(this) != original) revert CannotBeDelegateCalled();

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/NoDelegateCall.sol#L30

```solidity
267: if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;
271: if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;
274: uint256 shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;
293: if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);
295: if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);
297: Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L267

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L271

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L274

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L293

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L295

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L297



```solidity
393: if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;
397: if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;
411: if (long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);
413: if (long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L393

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L397

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L411

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L413



```solidity
444: uint256 balanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;
461: Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), balanceTarget);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L444

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L461



```solidity
482: address(this),
511: ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), param.isLong0ToLong1 ? TimeswapV2OptionPosition.Long0 : TimeswapV2OptionPosition.Long1),

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L482

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L511



```solidity
65: pair = deploy(address(this), optionPair, transactionFee, protocolFee);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L65

```solidity
125: uint160 liquidityBalanceTarget = ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)) + param.liquidityAmount;
143: Error.checkEnough(ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)), liquidityBalanceTarget);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L125

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L143



```solidity
87: long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + param.long0Amount;
117: long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + param.long1Amount;
146: shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + param.shortAmount;
186: if (param.long0Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0), long0BalanceTarget);
189: if (param.long1Amount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1), long1BalanceTarget);
192: if (param.shortAmount != 0) Error.checkEnough(ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short), shortBalanceTarget);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L87

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L117

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L146

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L186

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L189

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L192





#### <ins>Recommended Mitigation Steps</ins>

Use hardcoded `address`




### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

#### <ins>Proof Of Concept</ins>


```solidity
23: mapping(address => uint256) long0;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L23

```solidity
24: mapping(address => uint256) long1;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L24

```solidity
25: mapping(address => uint256) short;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L25

For the above, can create a struct for example:
```solidity
struct structShortLong{
	uint256 long0;
	uint256 long1;
	uint256 short;
}





### <a href="#Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> Optimize names to save gas

Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

See more <a href="https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92">here</a>

#### <ins>Proof Of Concept</ins>

All in-scope contracts

#### <ins>Recommended Mitigation Steps</ins>
Find a lower method ID name for the most called functions for example Call() vs. Call1() is cheaper by 22 gas
For example, the function IDs in the Gauge.sol contract will be the most used; A lower method ID may be given.



### <a href="#Summary">[GAS&#x2011;7]</a><a name="GAS&#x2011;7"> `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables

#### <ins>Proof Of Concept</ins>


```solidity
163: productA1 += (quotient1 * divisor);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/FullMath.sol#L163

```solidity
190: option.long0[msg.sender] -= token0AndLong0Amount;
191: option.long1[msg.sender] -= token1AndLong1Amount;
192: option.short[msg.sender] -= shortAmount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L190-L192

```solidity
237: if (param.isLong0ToLong1) option.long0[msg.sender] -= token0AndLong0Amount;
238: else option.long1[msg.sender] -= token1AndLong1Amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L237-L238

```solidity
277: option.short[msg.sender] -= shortAmount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L277

```solidity
62: option.long0[msg.sender] -= amount;
63: option.long0[to] += amount;
65: option.long1[to] += amount;
66: option.long1[msg.sender] -= amount;
68: option.short[msg.sender] -= amount;
69: option.short[to] += amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L62

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L63

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L65

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L66

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L68

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L69



```solidity
103: option.totalLong0 += token0AndLong0Amount;
104: option.long0[long0To] += token0AndLong0Amount;
108: option.totalLong1 += token1AndLong1Amount;
109: option.long1[long1To] += token1AndLong1Amount;
112: option.short[shortTo] += shortAmount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L103

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L104

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L108

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L109

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L112



```solidity
141: option.totalLong0 -= token0AndLong0Amount;
142: option.totalLong1 -= token1AndLong1Amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L141-L142

```solidity
173: option.totalLong0 -= token0AndLong0Amount;
174: option.totalLong1 += token1AndLong1Amount;
175: option.long1[longTo] += token1AndLong1Amount;
177: option.totalLong1 -= token1AndLong1Amount;
178: option.totalLong0 += token0AndLong0Amount;
179: option.long0[longTo] += token0AndLong0Amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L173

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L174

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L175

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L177

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L178

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L179



```solidity
208: option.totalLong0 -= token0Amount;
209: option.totalLong1 -= token1Amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L208-L209

```solidity
149: if (isAdd) longAmount -= fees;
150: else shortAmount -= fees;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L149-L150

```solidity
180: shortAmount -= fees;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L180

```solidity
212: longAmount -= fees;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L212

```solidity
244: amount -= fees;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L244

```solidity
295: denominator2 += longAmount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L295

```solidity
55: liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth);
56: liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth);
57: liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L55-L57

```solidity
66: liquidityPosition.liquidity += liquidityAmount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L66

```solidity
70: liquidityPosition.long0Fees += long0Fees;
71: liquidityPosition.long1Fees += long1Fees;
72: liquidityPosition.shortFees += shortFees;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L70-L72

```solidity
76: liquidityPosition.liquidity -= liquidityAmount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L76

```solidity
80: liquidityPosition.long0Fees -= long0Fees;
81: liquidityPosition.long1Fees -= long1Fees;
82: liquidityPosition.shortFees -= shortFees;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L80-L82

```solidity
361: if (long0Amount != 0) pool.long0Balance += long0Amount;
362: if (long1Amount != 0) pool.long1Balance += long1Amount;
365: pool.liquidity += liquidityAmount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L361

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L362

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L365



```solidity
452: if (long0Amount != 0) pool.long0Balance -= long0Amount;
453: if (long1Amount != 0) pool.long1Balance -= long1Amount;
455: pool.liquidity -= liquidityAmount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L452

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L453

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L455



```solidity
537: if (long0Amount != 0) pool.long0Balance += long0Amount;
538: if (long1Amount != 0) pool.long1Balance += long1Amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L537-L538

```solidity
636: pool.long0Balance -= (long0Amount + long0Fees);
650: pool.long1Balance -= (long1Amount + long1Fees);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L636

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L650



```solidity
677: pool.long1Balance -= (long1Amount + longFees);
677: pool.long1Balance -= (long1Amount + longFees);
695: pool.long0Balance += long0Amount;
710: pool.long0Balance -= (long0Amount + longFees);
710: pool.long0Balance -= (long0Amount + longFees);
722: pool.long1Balance += long1Amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L677

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L677

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L695

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L710

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L710

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L722



```solidity
61: _idTotalSupply[id] += amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L61

```solidity
95: _idTotalSupply[id] -= amount;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L95

```solidity
117: _currentIndex[to] += 1;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L117

```solidity
31: feesPosition.long0Fees += FeeCalculation.getFees(liquidity, feesPosition.long0FeeGrowth, long0FeeGrowth);
32: feesPosition.long1Fees += FeeCalculation.getFees(liquidity, feesPosition.long1FeeGrowth, long1FeeGrowth);
33: feesPosition.shortFees += FeeCalculation.getFees(liquidity, feesPosition.shortFeeGrowth, shortFeeGrowth);

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/FeesPosition.sol#L31-L33

```solidity
53: feesPosition.long0Fees += long0Fees;
54: feesPosition.long1Fees += long1Fees;
55: feesPosition.shortFees += shortFees;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/FeesPosition.sol#L53-L55

```solidity
59: feesPosition.long0Fees -= long0Fees;
60: feesPosition.long1Fees -= long1Fees;
61: feesPosition.shortFees -= shortFees;

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/FeesPosition.sol#L59-L61





### <a href="#Summary">[GAS&#x2011;8]</a><a name="GAS&#x2011;8"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```solidity
function positionOf(address owner, TimeswapV2TokenPosition calldata timeswapV2TokenPosition) public view returns (uint256 amount) {
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L66





### <a href="#Summary">[GAS&#x2011;9]</a><a name="GAS&#x2011;9"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```solidity
205: uint160 deltaRate;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L205

```solidity
12: uint96 internal constant NOT_INTERACTED = 0;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L12

```solidity
15: uint96 internal constant NOT_ENTERED = 1;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L15

```solidity
18: uint96 internal constant ENTERED = 2;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L18

```solidity
72: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/CallbackParam.sol#L72

```solidity
72: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/CallbackParam.sol#L72

```solidity
72: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/CallbackParam.sol#L72

```solidity
16: uint160 liquidity;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L16

```solidity
52: uint160 liquidity = liquidityPosition.liquidity;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L52

```solidity
42: uint160 liquidity;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L42

```solidity
43: uint96 lastTimestamp;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L43

```solidity
44: uint160 sqrtInterestRate;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L44

```solidity
125: uint160 liquidityBalanceTarget = ITimeswapV2Pool(poolPair).liquidityOf(param.strike, param.maturity, address(this)) + param.liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L125

```solidity
70: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L70

```solidity
70: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L70

```solidity
70: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L70

```solidity
90: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L90

```solidity
90: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L90

```solidity
90: uint160 liquidityAmount;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L90





### <a href="#Summary">[GAS&#x2011;10]</a><a name="GAS&#x2011;10"> Use solidity version 0.8.17 to gain some gas boost

Upgrade to the latest solidity version 0.8.17 to get additional gas savings.

#### <ins>Proof Of Concept</ins>


```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/BytesLib.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/CatchError.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Error.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/FullMath.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Math.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/Ownership.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/SafeCast.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-library/src/StrikeConversion.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/NoDelegateCall.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2Option.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/enums/Position.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/enums/Transaction.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/libraries/OptionFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/libraries/OptionPair.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/libraries/Proportion.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Option.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Param.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/Process.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-option/src/structs/StrikeAndMaturity.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/NoDelegateCall.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2Pool.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/enums/Transaction.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ConstantSum.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/Duration.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/DurationCalculation.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/DurationWeight.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/Fee.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/PoolFactory.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/PoolPair.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Param.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-pool/src/structs/Pool.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2Token.sol#L2

```solidity
pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L4

```solidity
pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L4

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol#L2

```solidity
pragma solidity =0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2TokenMintCallback.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/CallbackParam.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/FeesPosition.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Param.sol#L2

```solidity
pragma solidity ^0.8.8;
```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs/Position.sol#L2






### <a href="#Summary">[GAS&#x2011;11]</a><a name="GAS&#x2011;11"> Using `storage` instead of `memory` saves gas

When fetching data from a `storage` location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from `storage`, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another `memory` array/struct

#### <ins>Proof Of Concept</ins>


```solidity
238: TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition = _timeswapV2LiquidityTokenPositions[id];
264: TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition = _timeswapV2LiquidityTokenPositions[id];

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L238

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L264



```solidity
238: TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition = _timeswapV2LiquidityTokenPositions[id];
264: TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition = _timeswapV2LiquidityTokenPositions[id];
275: FeesPosition memory feesPosition = _feesPositions[id][owner];

```

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L238

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L264

https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L275





