
## 1 . Zero check should be in first :-

in first line of sqrt() function it convert to type(uint256) to type(uint128) but 0 check is after that. So change it add first 0 check and after that you can add conversion of uint256 to uint128 .

code snippet:-

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L71

## 2 Owner check can be in first in function before any other operations  :-
Owner check can be in first in function before check params and raiseguard .

code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L189

## 3 .Lack of to_ address check :-
code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L158.
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L183.

##  4 .Add token contract check before creating pair.:-

function create() is there for create token pair but no token contract check . it's good practice to check contract before make a pair .

code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L43


## 5 .Wrong comment :-

In the mention comment says [/// @param optionPair The address of the existed Pair contract. ] But when caller calls with exist optionPair address it reverts the transaction because of this line :-
        if (optionPair != address(0)) revert OptionPairAlreadyExisted(token0, token1, optionPair); 

code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionPair.sol#L37

Recommendation :-
Change the comment like this 
/// @param optionPair  need address(0)  or else it will revert.

## 6 . No array check of _ids and _accounts  :-

code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L224

Recommendation :-
Add require( uint256[] memory ids == uint256[] memory amounts,"No match");