### Floating Pragma
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Lock the pragma version and also consider [known bugs](https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.

Pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or EthPM package. Otherwise, the developer would need to manually update the pragma in order to compile locally.

*Instances (6):*
```
File: packages\v2-token\src\structs\FeesPosition.sol
2:    pragma solidity ^0.8.8;
```
[Link to code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol)
```
File: packages\v2-token\src\structs\Param.sol
2:    pragma solidity ^0.8.8;
```
[Link to code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol)
```
File: packages\v2-token\src\structs\Position.sol
2:    pragma solidity ^0.8.8;
```
[Link to code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol)
```
File: packages\v2-token\src\structs\CallbackParam.sol
2:    pragma solidity ^0.8.8;
```
[Link to code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol)
```
File: packages\v2-token\src\TimeswapV2LiquidityToken.sol
2:    pragma solidity ^0.8.8;
```
[Link to code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol)
```
File: packages\v2-token\src\TimeswapV2Token.sol
2:    pragma solidity ^0.8.8;
```
[Link to code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol)

*Recommendation:*
Consider locking pragma to a specific version (0.8.8).
