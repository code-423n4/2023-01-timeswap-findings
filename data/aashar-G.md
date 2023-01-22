### Gas Optimization

#### Unused Imports

| Contract | Line Number | Unused Import |
|----------|-------------|---------------|
| timeswapV2Token.sol | 5 | console.sol |
| TimeswapV2Pool.sol | 34 | Transactions.sol |
| TimeswapV2Pool.sol | 21 | ITimeswapV2PoolBurnCallback |
| TimeswapV2Pool.sol | 32 | TimeswapV2PoolMintChoiceCallbackParam, TimeswapV2PoolBurnChoiceCallbackParam, TimeswapV2PoolDeleverageChoiceCallbackParam and TimeswapV2PoolLeverageChoiceCallbackParam |
| Pool.sol | 8 | ITimeswapV2Option |
| Pool.sol | 25 | TransactionLibrary |
| Pool.sol | 27 | TimeswapV2PoolCollectParam |
| DurationCalculation.sol | 7 | DurationLibrary |
| ConstantSum.sol | 8 | Error.sol |
| TimeswapV2Option.sol | 24 | Transaction.sol |
| Option.sol | 5 | Error.sol |

#### Cloning contracts with EIP 1167

Usage of eip 1167 for deploying new contract in TimeswapV2PoolDeployer.sol and also in TimeswapV2OptionDeployer.sol. 

#### Unused File

Duration.sol is not used anywhere in the contracts other than the test files.