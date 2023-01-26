## QA Issues List
| Number |Issues Details
|:--:|:-------|:--:|
|[QA-01]| Use latest Solidity version
|[QA-02]| Use stable pragma statement
|[QA-03]| Different pragma directives are used
|[QA-04]|Update external dependency to latest version
|[QA-05]|In all solidity files, license keyword should be mentioned
|[QA-06]| Solidity compiler optimizations can be problematic
|[QA-07]|Use named inports instead of plain import `FILE.SOL`
* * *

## [QA-01] Use latest Solidity version

Solidity pragma versioning should be upgraded to latest available version. 
***

## [QA-02] Use stable pragma statement

Using a floating pragma statement is discouraged as code can compile to different bytecodes with different compiler versions. Use a stable pragma statement to get a deterministic bytecode.
```
packages/v2-token/src/interfaces/IERC1155Enumerable.sol:
4: pragma solidity ^0.8.0;

packages/v2-token/src/base/ERC1155Enumerable.sol:
4: pragma solidity ^0.8.0;

packages/v2-token/src/TimeswapV2LiquidityToken.sol:
2: pragma solidity ^0.8.8;

packages/v2-token/src/TimeswapV2Token.sol:
2: pragma solidity ^0.8.8;

packages/v2-token/src/structs/*
pragma solidity ^0.8.8;


```
***

## [QA-03] Different pragma directives are used

Use one Solidity version on each contract.
***

## [QA-04] Update external dependency to latest version

The leatest version is 4.8.1.

```
v2-library/package.json:
	40: "@openzeppelin/contracts": "^4.7.2"
	
v2-pool/package.json:
	41: "@openzeppelin/contracts": "^4.8.0"

v2-token/package.json:
	40: "@openzeppelin/contracts": "^4.7.2"
```
***

## [QA-05] In all solidity files, license keyword should be mentioned
***

## [QA-06] Solidity compiler optimizations can be problematic
```
v2-pool/hardhat.config.ts, v2-option/hardhat.config.ts and v2-token/hardhat.config.ts:

solidity: {
    version: "0.8.8",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
 ```
 
Protocol has enabled optional compiler optimizations in Solidity. There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them.
***

## [QA-07] USE NAMED IMPORTS INSTEAD OF PLAIN `IMPORT ‘FILE.SOL’
```
packages/v2-token/src/interfaces/IERC1155Enumerable.sol:

import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
```