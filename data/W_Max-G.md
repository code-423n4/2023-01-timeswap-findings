# writing before all

1. All the `require` and `assert` should not be used due to gas issues. I suggest using custom errors instead.
2. I suggest a higher level of solidity (0.8.17)

# v2-library

## CatchError.sol

1. in line 15:
   `(length - 4) % 32 == 0` can be replaced by: 
   `length % 32 == 4` to save gas.

# v2-pool

## enums

### transaction.sol

1. in line: 53,58,63,68,73:
   `uint256(transaction) >= 4` should be replaced with `uint256(transaction) > 3` to save gas.

## struct

### LiquidityPosition
1. in line 87-89: 
   `uint256 long0Requested`
   `uint256 long1Requested`
   `uint256 shortRequested`
   should be cached to save gas as they are used for more thanm twice in the function.

### Pool.sol
1. in line 41: this struct does not take full use of slots. (as all the bits sum together % 256 != 0) can be changed to use slots fully to save gas.
2. in line 130: `pool.lastTimestamp` should be cached to save gas.
3. in line 183: `uint256 maturity, uint256 long0Fees, uint256 long1Fees, uint256 shortFees, uint96 blockTimestamp` should be cached as the are used more than once to save gas.
4. in line 264: `maturity` should be cached to save gas as it is used for more than once.
5. in line 555: `uint256 transactionFee, uint256 protocolFee, uint96 blockTimestamp` should be cached to save gas as they are used for more than once.

### Param.sol
1. in line 136: `&&` should not be used to save gas. (Can be saperated)
2. in line 51: `uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth` should be cached to save gas as they are used for more than once
3. in l;ine 87: `uint256 long0Requested,uint256 long1Requested,uint256 shortRequested` should be cached to save gas as they are used for more than once

### OwnableTwoSteps.sol
1. in line 31: `pendingOwner` should not be used in emit, use memory variables instead.

## TimeswapV2Pool
1. in line 167: `&&` should not be used. can be saperated to save gas.
   
2. in line 52: 
   `    mapping(uint256 => mapping(uint256 => uint96)) private reentrancyGuards;`
    There are lots of mappings using type uint96 instead of uint256, this is more gas-cost than using uint256

3. in line 57-60, line 66-69:
    `strike` and `maturity` of calldata are used more than once so should be cacheed to save gas.

4. in line 152:
   `uint256 strike, uint256 maturity, address to, uint160 liquidityAmount`
   these variables should all be cached as they are used more than once in the function.

5. in line 165:
   `uint256 strike, uint256 maturity, address to, uint256 long0Fees, uint256 long1Fees, uint256 shortFees`
   these variables should all be cached as they are used more than once in the function.

6. in line 175:
   `uint256 strike, uint256 maturity, uint160 rate`
   these variables should all be cached as they are used more than once in the function.

6. in line 184,202, 253,318, 378, 431, 469:
   vars in `param` should be cached to save gas as they are used more than once in the function.

7. in line 231:
   `uint256 strike, uint256 maturity, address long0To, address long1To, address shortTo, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount`
   these variables should all be cached as they are used more than once in the function.

## libraries

### Fee.sol
1. in line 27: `fees` is used more than once in the function so should be cached.

# v2-token

## TimeswapV2LiquidityToken.sol

1. in line 41: `mapping(bytes32 => uint96) private reentrancyGuards;` costs more gas than using uint256 type maps.
2. in line 75: `address from, address to, TimeswapV2LiquidityTokenPosition calldata position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees` should be cached to save gas.
3. in line 150, : variable s in `param` should be cached to save gas.
4. in line 236: `address from, address to, uint256 id` shoule be cached to save gas.
5. in line 263: `(address owner, uint256 id)` shoule be cached to save gas.

