## 1 . Use named returns for local variables where it is possible :-

code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L12

## 2 . Using Solidity version 0.8.17 will provide an overall gas optimization

Using at least 0.8.10 will save gas due to skipped extcodesize check if there is a return value. Currently the contracts are compiled using version 0.8.7 (Foundry default). It is easily changeable to 0.8.17 using the command sed -i 's/0\.8\.7/^0.8.0/' test/*.sol && sed -i '4isolc = "0.8.17"

code snippet:-
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L2
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol#L2

And all scoped files


