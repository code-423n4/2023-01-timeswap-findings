# QA Report

## General Comments
- Overall the codebase is well-written and intention is generally clear. Variable and function names are descriptive, the code appears to have been written with security issues like reentrancy, overflows, etc. in mind.
- I would personally recommend against the practice of including extensive libraries in struct and enum files. Some files - e.g. `v2-pool/src/structs/Param.sol`, `v2-pool/src/structs/Pool.sol`, and `/v2-option/src/enums/Transaction.sol` - include both struct/enum declarations as well as a library for manipulation. These libraries would be better suited in `/libraries/`. 
- Tests are mostly lacking informational comments and still contain many commented-out lines of code


## Low Severity #1 - Newly-created options/pools never pushed to getByIndex array
The dynamic address[] array getByIndex in TimeswapV2OptionFactory.sol and TimeswapV2PoolFactory.sol is never pushed to when a new option/pool is created. This will result in the numberOfPairs() functions in the option/pool factories always returning 0.

- [Option Factory LOC](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L25)
- [Pool Factory LOC](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L33)
- [Errant Pool Test LOC - note that the assertEq() is checking that the number of created pools is 0, even after a pool is created](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/TimeswapV2PoolFactory.t.sol#L47)