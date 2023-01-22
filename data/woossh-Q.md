https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L231-L237

TimeswapV2Pool.sol: The collect() function does not check if there is a zero address in the long0To, long1To, shortTo variables. This could send the tokens to a null address resulting in loss of funds.

