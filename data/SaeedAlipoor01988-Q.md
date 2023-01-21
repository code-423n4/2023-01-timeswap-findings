Missing validation in constructors
TimeswapV2Token misses zero-address validation on the address of the chosenOptionFactory set in the constructor.
There is no other function to change the address of chosenOptionFactory in the TimeswapV2Token contract.
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L42

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36
The same is found here:
https://github.com/code-423n4/2022-01-trader-joe-findings/issues/263
https://github.com/code-423n4/2022-01-sandclock-findings/issues/107
////////////////////////////////////////////// ***** //////////////////////////////////////////////
Missing same address check
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L75
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L152

////////////////////////////////////////////// ***** //////////////////////////////////////////////

in TimeswapV2PoolFactory contract and function create, you are returning one error for two separate subjects. 
in the first line, if the input address is zero address, the function will return Error.zeroAddress();
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L60

but in the next line, we get pair from pairs mapping, and if the pair is zero address again function will return Error.zeroAddress();

but in this line, I think we should return this error, Error.pairalreadyexist();