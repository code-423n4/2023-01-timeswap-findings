# Gas Optimizations list

| Number | Details                                                                                            | Instances |
| ------ | -------------------------------------------------------------------------------------------------- | --------- |
| 1      | ```x += y``` costs more gas than ```x = x + y```. ```x -= y``` costs more gas than ```x = x - y``` | 73        |
| 2      | Can declare the variable outside the loop to save gas                                              | 4         |
| 3      | Using local arguments instead of state variable saves gas                                          | 1         |
| 4      | Expression can be `unchecked` when overflow is not possible                                        | 3         |
| 5      | Avoid contract existence checks by using low level calls                                           | 2         |
| 6      | Multiple address/id mappings can be combined into a single mapping to a struct                     | 5         |
| 7      | Length of bytes array can be cached outside require                                                | 1         |
| 8      | Public functions not called by contract can be changed to external to save some gas                | 1         |


# Gas Optimizations
## [G-01] ```x += y``` costs more gas than ```x = x + y```. ```x -= y``` costs more gas than ```x = x - y```
Using the addition/subtraction operators instead of plus-equals/minus-equals saves gas.


>packages\v2-library\src\FullMath.sol:
>```js
>163:             productA1 += (quotient1 * divisor);
>```
>
>packages\v2-option\src\structs\Option.sol:
>```js
>62              option.long0[msg.sender] -= amount;
>63:             option.long0[to] += amount;
>65:             option.long1[to] += amount;
>66              option.long1[msg.sender] -= amount;
>68              option.short[msg.sender] -= amount;
>69:             option.short[to] += amount;
>103:             option.totalLong0 += token0AndLong0Amount;
>104:             option.long0[long0To] += token0AndLong0Amount;
>108:             option.totalLong1 += token1AndLong1Amount;
>109:             option.long1[long1To] += token1AndLong1Amount;
>112:         option.short[shortTo] += shortAmount;
>141:         option.totalLong0 -= token0AndLong0Amount;
>142:         option.totalLong1 -= token1AndLong1Amount;
>173              option.totalLong0 -= token0AndLong0Amount;
>174:             option.totalLong1 += token1AndLong1Amount;
>175:             option.long1[longTo] += token1AndLong1Amount;
>177              option.totalLong1 -= token1AndLong1Amount;
>178:             option.totalLong0 += token0AndLong0Amount;
>179:             option.long0[longTo] += token0AndLong0Amount;
>208:         option.totalLong0 -= token0Amount;
>209:         option.totalLong1 -= token1Amount;
>```
>
>packages\v2-pool\src\libraries\ConstantProduct.sol:
>```js
>149:         if (isAdd) longAmount -= fees;
>150:         else shortAmount -= fees;
>180:             shortAmount -= fees;
>212:             longAmount -= fees;
>244:         amount -= fees;
>295:             denominator2 += longAmount;
>```
>
>packages\v2-pool\src\structs\LiquidityPosition.sol:
>```js
>55:             liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth);
>56:             liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth);
>57:             liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth);
>66:         liquidityPosition.liquidity += liquidityAmount;
>70:         liquidityPosition.long0Fees += long0Fees;
>71:         liquidityPosition.long1Fees += long1Fees;
>72:         liquidityPosition.shortFees += shortFees;
>76:         liquidityPosition.liquidity -= liquidityAmount;
>80:         liquidityPosition.long0Fees -= long0Fees;
>81:         liquidityPosition.long1Fees -= long1Fees;
>82:         liquidityPosition.shortFees -= shortFees;
>```
>
>packages\v2-pool\src\structs\Pool.sol:
>```js 
>361:         if (long0Amount != 0) pool.long0Balance += long0Amount;
>362:         if (long1Amount != 0) pool.long1Balance += long1Amount;
>365:         pool.liquidity += liquidityAmount; 
>537:         if (long0Amount != 0) pool.long0Balance += long0Amount;
>538:         if (long1Amount != 0) pool.long1Balance += long1Amount;
>695:             pool.long0Balance += long0Amount;
>722:             pool.long1Balance += long1Amount;
>452:         if (long0Amount != 0) pool.long0Balance -= long0Amount;
>453:         if (long1Amount != 0) pool.long1Balance -= long1Amount;
>455:         pool.liquidity -= liquidityAmount;
>636:                 pool.long0Balance -= (long0Amount + long0Fees);
>650:                 pool.long1Balance -= (long1Amount + long1Fees);
>677:                 pool.long1Balance -= (long1Amount + longFees);
>689:                     pool.long1Balance -= (long1Amount + longFees);
>710:                     pool.long0Balance -= (long0Amount + longFees);
>719:                 pool.long0Balance -= (long0Amount + longFees);
>```
>
>packages\v2-token\src\base\ERC1155Enumerable.sol:
>```js
>61:             _idTotalSupply[id] += amount;
>95:             _idTotalSupply[id] -= amount;
>117:         _currentIndex[to] += 1;
>```
>
>packages\v2-token\src\structs\FeesPosition.sol:
>```js
>31:             feesPosition.long0Fees += FeeCalculation.getFees(liquidity, feesPosition.long0FeeGrowth, long0FeeGrowth);
>32:             feesPosition.long1Fees += FeeCalculation.getFees(liquidity, feesPosition.long1FeeGrowth, long1FeeGrowth);
>33:             feesPosition.shortFees += FeeCalculation.getFees(liquidity, feesPosition.shortFeeGrowth, shortFeeGrowth);
>53:         feesPosition.long0Fees += long0Fees;
>54:         feesPosition.long1Fees += long1Fees;
>55:         feesPosition.shortFees += shortFees;
>59:         feesPosition.long0Fees -= long0Fees;
>60:         feesPosition.long1Fees -= long1Fees;
>61:         feesPosition.shortFees -= shortFees;
>```
>
>packages\v2-option\src\TimeswapV2Option.sol: 
>```js
>190:         option.long0[msg.sender] -= token0AndLong0Amount;
>191:         option.long1[msg.sender] -= token1AndLong1Amount;
>192:         option.short[msg.sender] -= shortAmount;
>237:         if (param.isLong0ToLong1) option.long0[msg.sender] -= token0AndLong0Amount;
>238:         else option.long1[msg.sender] -= token1AndLong1Amount;
>277:         option.short[msg.sender] -= shortAmount;
>```
>

## [G-02] Can declare the variable outside the loop to save gas
Declaring the loop variable outside the loop saves gas.

>packages\v2-option\src\structs\Process.sol:
>```js
>34:         for (uint256 i; i < processing.length; ) {
>```
>
>packages\v2-token\src\TimeswapV2LiquidityToken.sol:
>```js
>227:         for (uint256 i; i < ids.length; ) {
>```
>packages\v2-token\src\base\ERC1155Enumerable.sol:
>```js
>48:         for (uint256 i; i < ids.length; ) {
>82:         for (uint256 i; i < ids.length; ) {
>```

## [G-03] Using local arguments instead of state variable saves gas
Using the local arguments of the function while emitting an event instead of the state variable saves roughly 122 gas per emit.
>packages\v2-pool\src\base\OwnableTwoSteps.sol
>```js
>29:         pendingOwner = chosenPendingOwner;
>30: 
>31:         emit SetOwner(pendingOwner);
>```
>can be refactored to
>```js
>29:         pendingOwner = chosenPendingOwner;
>30: 
>31:         emit SetOwner(chosenPendingOwner);
>```

## [G-04] Expression can be `unchecked` when overflow is not possible
Using `unchecked` for an expression reduces the amount of opcodes required to check for overflow/underflow hence saving gas. 
>packages\v2-library\src\Math.sol
>
>```js
>48:     function div(uint256 dividend, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
>49:         quotient = dividend / divisor;
>50: 
>51:         if (roundUp && dividend % divisor != 0) quotient++;
>52:     }
>
>59:     function shr(uint256 dividend, uint8 divisorBit, bool roundUp) internal pure returns (uint256 quotient) {
>60:         quotient = dividend >> divisorBit;
>61: 
>62:         if (roundUp && dividend % (1 << divisorBit) != 0) quotient++;
>63:     }
> 
>81:         if (roundUp && value % result != 0) result++;
>```
>Dividing by 0 results in a revert. Since dividing any divisor by the dividend results in a quotient that is less than or equal to the dividend, it can be safely unchecked to save gas.

## [G-05] Avoid contract existence checks by using low level calls
Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

>packages\v2-token\src\TimeswapV2LiquidityToken.sol:
>```js
>70      function transferTokenPositionFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata timeswapV2LiquidityTokenPosition, uint160 liquidityAmount) external {
>71:         safeTransferFrom(from, to, _timeswapV2LiquidityTokenPositionIds[timeswapV2LiquidityTokenPosition.toKey()], liquidityAmount, bytes(""));
>72      }
>```
>packages\v2-token\src\TimeswapV2Token.sol:
>```js
>71      function transferTokenPositionFrom(address from, address to, TimeswapV2TokenPosition calldata timeswapV2TokenPosition, uint256 amount) external override {
>72:         safeTransferFrom(from, to, _timeswapV2TokenPositionIds[timeswapV2TokenPosition.toKey()], (amount), bytes(""));
>73      }
>```

## [G-06] Multiple address/id mappings can be combined into a single mapping to a struct
If both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. Depending on the circumstances and sizes of types, can avoid a Gsset per mapping combined. 

>File: packages\v2-token\src\base\ERC1155Enumerable.sol
>```js
>14:     // Mapping from owner to list of owned token IDs
>15:     mapping(address => mapping(uint256 => uint256)) private _ownedTokens; // An index of all tokens
>16: 
>17:     mapping(address => uint256) private _currentIndex; // the current index for an address
>18: 
>19:     // Mapping from token ID to index of the owner tokens list
>20:     mapping(uint256 => uint256) private _ownedTokensIndex;
>21: 
>22:     mapping(uint256 => uint256) private _idTotalSupply;
>23: 
>24:     // Array with all token ids, used for enumeration
>25:     uint256[] private _allTokens;
>26: 
>27:     // Mapping from token id to position in the allTokens array
>28:     mapping(uint256 => uint256) private _allTokensIndex;
>```
>
>In this contract the address mappings could be combined into one struct and the id mappings into another struct. The values could be cached into the stack and used for simple reads when required. 

## [G-07] Length of bytes array can be cached outside require
Length of the dynamic bytes array can be cached outside the require statement to save a small amount of gas. 

>packages\v2-library\src\BytesLib.sol:
>```js
>12      function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {
>13:         require(_length + 31 >= _length, "slice_overflow");
>14:         require(_bytes.length >= _start + _length, "slice_outOfBounds");
>```

## [G-08] Public functions not called by contract can be changed to external to save some gas
Contracts are allowed to override their parents’ functions and change the visibility from external to public and can save gas by doing so.

>packages\v2-token\src\base\ERC1155Enumerable.sol:
>```js
>35      /// @inheritdoc IERC1155Enumerable
>36:     function totalSupply() public view override returns (uint256) {
>37          return _allTokens.length;
>```

