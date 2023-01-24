### Remove the Contradictory If statement to check for maturity in external function transferLiquidity() found in TimeswapV2Pool.sol it is not needed according to what is stated in the whitepaper. I.e liquidity providers can withdraw the liquidity whenever they like (before or after maturity). 
### This is also found in function initialise() in TimeswapV2Pool.sol also

* Details: # Lines of code https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L155 #

* Details: # Lines of code https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2Pool.sol#L176 