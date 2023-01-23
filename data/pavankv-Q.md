## 1 .Use Short Circuiting rules to your advantage
The operators || and && apply the common short-circuiting rules. This means that in the expression f(x) || g(y), if f(x) evaluates to true, g(y) will not be evaluated even if it may have side-effects.
So setting less costly function to “f(x)” and setting costly function to “g(x)” is efficient.

In below code snippet dev use  || to check addess(0) if one true it just give error but not look into other two .

code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L134
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L156
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L179

Change to :-
        if (param.long0To == address(0) && param.long1To == address(0) && param.shortTo == address(0)) Error.zeroAddress();


## 2 . Zero check should be in first :-

code snippet:-

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L71

## 3 Owner check can be in first in function  :-
Owner check can be in first in function before check params and raiseguard .

code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L189

## 4 .Lack of to_ address check :-
code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L158.
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L183.



