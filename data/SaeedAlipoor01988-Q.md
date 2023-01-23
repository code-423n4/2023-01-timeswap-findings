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
in the first line, if the input address is zero address, the function will return an Error.zeroAddress();
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L60

but in the next line, we get pair from pairs mapping, and if the pair is zero address again function will return Error.zeroAddress();

but in this line, I think we should return this error, Error.pairalreadyexist();

////////////////////////////////////////////// ***** //////////////////////////////////////////////

in transferLiquidity function,
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L152

user A wants to transfer liquidityAmount from his balance to user B's balance. but we don't check at top of the function body that, user A has enough liquidity and directly we call 

pools[strike][maturity].transferLiquidity(to, liquidityAmount, blockTimestamp(0));

but we can manage this scenario more easily,

liquidityAmountToTransfer = 
liquidityAmount > pools[strike][maturity].liquidityPositions[msg.sender].liquidity ? pools[strike][maturity].liquidityPositions[msg.sender].liquidity : liquidityAmount ;

and use liquidityAmountToTransfer in burn and mint, instead of using the user's input liquidityAmount .

same for :
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L165

////////////////////////////////////////////// ***** //////////////////////////////////////////////

Omissions in Events with old value
Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

The events should include the new value and old value where possible:
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L41

////////////////////////////////////////////// ***** //////////////////////////////////////////////

in transferLiquidity function,
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L153

at the first line, we check that the matching pool has liquidity of more than zero.
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L98

then w will call transferLiquidity function from the pool library
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L158

again at the first line of this function, we have and if block to check if the pool has liquidity.

this is double-checking!

////////////////////////////////////////////// ***** //////////////////////////////////////////////