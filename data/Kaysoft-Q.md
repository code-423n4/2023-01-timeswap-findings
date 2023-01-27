## [NC-01] AVOID FLOATING PRAGMA

See: https://swcregistry.io/docs/SWC-103

Files: 
- [packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2)
- [packages/v2-token/src/TimeswapV2Token.sol#L2](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L2)

And all files in this struct directory: https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs

## [NC-02] REMOVE UNUSED IMPORTS

```
import "forge-std/console.sol";
```
`import "forge-std/console.sol";` was imported in the file: [packages/v2-token/src/TimeswapV2Token.sol#L5](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L5)

## [NC-03] NO NATSPEC COMMENTS ON SOME FUNCTIONS

FILES:
- [packages/v2-token/src/TimeswapV2LiquidityToken.sol#L236](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L236)
- [packages/v2-token/src/TimeswapV2LiquidityToken.sol#L255](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L255)


## [L-01] Solidity compiler optimization can be problematic
File: hardhat.config.ts
```
const config: HardhatUserConfig = {
  paths: {
    sources: "./src",
  },
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

### Description

Protocol has enabled optional compiler optimizations in Solidity. There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them.

Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG.

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported. A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe. It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

Exploit Scenario A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.

### Recommended Mitigation Steps

Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug.Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## [L-02] USE LATEST STABLE PRAGMA VERSION

See: https://swcregistry.io/docs/SWC-102
Files: all files
The project uses pragma version 0.8.8

#### Recommended Mitigation steps
Consider using latest solidity pragma version 0.8.17 since it will have updates and bugfixes.