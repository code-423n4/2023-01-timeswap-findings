## [L-01] SPDX-License-Identifier missing

### Description

Missing license agreements (SPDX-License-Identifier) may result in lawsuits and improper forms of use of code.

### Findings

- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC1155.sol => The Solidity file is missing the SPDX-License-Identifier
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC165.sol => The Solidity file is missing the SPDX-License-Identifier
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC20.sol => The Solidity file is missing the SPDX-License-Identifier
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC4626.sol => The Solidity file is missing the SPDX-License-Identifier
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC721.sol => The Solidity file is missing the SPDX-License-Identifier
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ProcessExt.sol => The Solidity file is missing the SPDX-License-Identifier

### References

- [Missing license identifier | UMA DVM 2.0 Audit](https://blog.openzeppelin.com/uma-dvm-2-0-audit/)


## [L-02] Compiler version Pragma is non-specific

### Description

For non-library contracts, floating pragmas may be a security risk for application implementations, since a known vulnerable compiler version may accidentally be selected or security tools might fallback to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

### Findings

- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Ownership.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/SafeCast.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/BytesLibExt.sol => `pragma solidity ^0.8.0;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/CatchErrorExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/ErrorExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/FullMathExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/MathExt.sol => `pragma solidity ^0.8.0;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/SafeCastExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/StrikeConversionExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/NoDelegateCall.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Position.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/CallbackParam.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/StrikeAndMaturity.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/OptionFactoryExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/OptionPairExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ParamExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ProportionExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/NoDelegateCall.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/ConstantProductExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationCalculationExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationWeightExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/FeeExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/PoolFactoryExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/PoolPairExt.sol => `pragma solidity =0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol => `pragma solidity ^0.8.8;`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol => `pragma solidity ^0.8.8;`

### References

- [L003 - Unspecific Compiler Version Pragma](https://github.com/byterocket/c4-common-issues/blob/main/2-Low-Risk.md#l003---unspecific-compiler-version-pragma)
- [Version Pragma | Solidity documents](https://docs.soliditylang.org/en/latest/layout-of-source-files.html#version-pragma)
- [4.6 Unspecific compiler version pragma | Consensys Audit of 1inch Liquidity Protocol](https://consensys.net/diligence/audits/2020/12/1inch-liquidity-protocol/#unspecific-compiler-version-pragma)


## [L-03] Outdated Compiler Version

### Description

Using an older compiler version might be risky, especially if the version in question has faults and problems that have been made public.

### Findings

- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/demo/demo.sol => `>=0.5.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/src/test.sol => `>=0.5.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/Common.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/Components.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/Script.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdAssertions.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdCheats.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdError.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdJson.sol => `>=0.6.0 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdMath.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdStorage.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdUtils.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/Test.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/Vm.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/console.sol => `>=0.4.22 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/console2.sol => `>=0.4.22 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC1155.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC165.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC20.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC4626.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC721.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/test/compilation/CompilationScript.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/test/compilation/CompilationScriptBase.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/test/compilation/CompilationTest.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/test/compilation/CompilationTestBase.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Ownership.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/SafeCast.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/BytesLibExt.sol => `^0.8.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/CatchErrorExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/ErrorExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/FullMathExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/MathExt.sol => `^0.8.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/SafeCastExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/StrikeConversionExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/NoDelegateCall.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Position.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionFactory.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionPair.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/Proportion.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/CallbackParam.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol => =0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/StrikeAndMaturity.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/OptionFactoryExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/OptionPairExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ParamExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ProportionExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/lib/ds-test/demo/demo.sol => `>=0.5.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/lib/ds-test/src/test.sol => `>=0.5.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/Base.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/Script.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdAssertions.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdChains.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdCheats.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdError.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdJson.sol => `>=0.6.0 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdMath.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdStorage.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdUtils.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/Test.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/Vm.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/console.sol => `>=0.4.22 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/console2.sol => `>=0.4.22 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/interfaces/IERC1155.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/interfaces/IERC165.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/interfaces/IERC20.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/interfaces/IERC4626.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/interfaces/IERC721.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/test/compilation/CompilationScript.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/test/compilation/CompilationScriptBase.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/test/compilation/CompilationTest.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/test/compilation/CompilationTestBase.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/NoDelegateCall.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/enums/Transaction.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantSum.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/Duration.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/DurationCalculation.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/DurationWeight.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/Fee.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolPair.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ReentrancyGuard.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/ConstantProductExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationCalculationExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationWeightExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/FeeExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/PoolFactoryExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/PoolPairExt.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/lib/ds-test/demo/demo.sol => `>=0.5.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/lib/ds-test/src/test.sol => `>=0.5.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/Base.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/InvariantTest.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/Script.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdAssertions.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdChains.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdCheats.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdError.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdJson.sol => `>=0.6.0 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdMath.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdStorage.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdUtils.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/Test.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/Vm.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/console.sol => `>=0.4.22 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/console2.sol => `>=0.4.22 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IERC1155.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IERC165.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IERC20.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IERC4626.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IERC721.sol => `>=0.6.2`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IMulticall3.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/test/compilation/CompilationScript.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/test/compilation/CompilationScriptBase.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/test/compilation/CompilationTest.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/test/compilation/CompilationTestBase.sol => `>=0.6.2 <0.9.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol => `^0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol => `^0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol => `^0.8.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol => `^0.8.0`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenBurnCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenCollectCallback.sol `=> =0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2TokenBurnCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2TokenMintCallback.sol => `=0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol => `^0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol => `^0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol => `^0.8.8`
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol => `^0.8.8`

### Resources

- [SWC-102](https://swcregistry.io/docs/SWC-102)
- [Etherscan Solidity Bug Info](https://etherscan.io/solcbuginfo)


## [L-03] For modern and more readable code; update import usages

### Description

A less obvious way that solidity code is clearer is the struct Point. Prior to now, we imported it via global import, but we didn't use it. The Point struct contaminated the source code with an extra object that was not needed and that we were not utilizing.

To be sure to only import what you need, use specific imports using curly brackets.

### Findings

- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/demo/demo.sol
  ```
  ::4 => import "../src/test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/Components.sol
  ```
  ::4 => import "./console.sol";
  ::5 => import "./console2.sol";
  ::6 => import "./StdAssertions.sol";
  ::7 => import "./StdCheats.sol";
  ::8 => import "./StdError.sol";
  ::9 => import "./StdJson.sol";
  ::10 => import "./StdMath.sol";
  ::11 => import "./StdStorage.sol";
  ::12 => import "./StdUtils.sol";
  ::13 => import "./Vm.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdAssertions.sol
  ```
  ::4 => import "ds-test/test.sol";
  ::5 => import "./StdMath.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdCheats.sol
  ```
  ::6 => import "./StdStorage.sol";
  ::7 => import "./Vm.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdJson.sol
  ```
  ::6 => import "./Vm.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdStorage.sol
  ```
  ::4 => import "./Vm.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdUtils.sol
  ```
  ::4 => import "./console2.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/Test.sol
  ```
  ::5 => import "ds-test/test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC1155.sol
  ```
  ::3 => import "./IERC165.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC4626.sol
  ```
  ::3 => import "./IERC20.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/interfaces/IERC721.sol
  ```
  ::3 => import "./IERC165.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/test/compilation/CompilationScript.sol
  ```
  ::6 => import "../../src/Script.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/test/compilation/CompilationScriptBase.sol
  ```
  ::6 => import "../../src/Script.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/test/compilation/CompilationTest.sol
  ```
  ::6 => import "../../src/Test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/test/compilation/CompilationTestBase.sol
  ```
  ::6 => import "../../src/Test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/BytesLibExt.sol
  ```
  ::4 => import "../../src/BytesLib.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/CatchErrorExt.sol
  ```
  ::3 => import "../../src/CatchError.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/ErrorExt.sol
  ```
  ::3 => import "../../src/Error.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/MathExt.sol
  ```
  ::4 => import "../../src/Math.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/SafeCastExt.sol
  ```
  ::3 => import "../../src/SafeCast.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/StrikeConversionExt.sol
  ```
  ::3 => import "../../src/StrikeConversion.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/OptionFactoryExt.sol
  ```
  ::4 => import "../../src/libraries/OptionFactory.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/OptionPairExt.sol
  ```
  ::4 => import "../../src/libraries/OptionPair.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ParamExt.sol
  ```
  ::4 => import "../../src/structs/Param.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ProcessExt.sol
  ```
  ::1 => import "../../src/structs/Process.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ProportionExt.sol
  ```
  ::4 => import "../../src/libraries/Proportion.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/lib/ds-test/demo/demo.sol
  ```
  ::4 => import "../src/test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/interfaces/IERC1155.sol
  ```
  ::4 => import "./IERC165.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/interfaces/IERC4626.sol
  ```
  ::4 => import "./IERC20.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/interfaces/IERC721.sol
  ```
  ::4 => import "./IERC165.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/test/compilation/CompilationScript.sol
  ```
  ::6 => import "../../src/Script.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/test/compilation/CompilationScriptBase.sol
  ```
  ::6 => import "../../src/Script.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/test/compilation/CompilationTest.sol
  ```
  ::6 => import "../../src/Test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/test/compilation/CompilationTestBase.sol
  ```
  ::6 => import "../../src/Test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/ConstantProductExt.sol
  ```
  ::4 => import "../../src/libraries/ConstantProduct.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationCalculationExt.sol
  ```
  ::4 => import "../../src/libraries/DurationCalculation.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationExt.sol
  ```
  ::4 => import "../../src/libraries/Duration.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationWeightExt.sol
  ```
  ::4 => import "../../src/libraries/DurationWeight.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/FeeExt.sol
  ```
  ::4 => import "../../src/libraries/Fee.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/PoolFactoryExt.sol
  ```
  ::4 => import "../../src/libraries/PoolFactory.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/PoolPairExt.sol
  ```
  ::4 => import "../../src/libraries/PoolPair.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/lib/ds-test/demo/demo.sol
  ```
  ::4 => import "../src/test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IERC1155.sol
  ```
  ::4 => import "./IERC165.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IERC4626.sol
  ```
  ::4 => import "./IERC20.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/interfaces/IERC721.sol
  ```
  ::4 => import "./IERC165.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/test/compilation/CompilationScript.sol
  ```
  ::6 => import "../../src/Script.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/test/compilation/CompilationScriptBase.sol
  ```
  ::6 => import "../../src/Script.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/test/compilation/CompilationTest.sol
  ```
  ::6 => import "../../src/Test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/test/compilation/CompilationTestBase.sol
  ```
  ::6 => import "../../src/Test.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol
  ```
  ::5 => import "forge-std/console.sol";
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol
  ```
  ::6 => import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
  ```

### Resources

- [[N-03] FOR MODERN AND MORE READABLE CODE; UPDATE IMPORT USAGES | PoolTogether contest](https://code4rena.com/reports/2022-12-pooltogether#n-03-for-modern-and-more-readable-code-update-import-usages)


## [L-04] Unnamed return parameters

### Description

To increase explicitness and readability, take into account introducing and utilizing named return parameters.

### Findings

- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/demo/demo.sol
  ```
  ::62 =>     function echo(string memory s1, string memory s2) public pure returns (string memory, string memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/src/test.sol
  ```
  ::50 =>     function failed() public returns (bool) {
  ::71 =>     function hasHEVMContext() internal view returns (bool) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdCheats.sol
  ```
  ::209 =>     function _constructor() private returns (uint256) {
  ::269 =>     function readEIP1559ScriptArtifact(string memory path) internal view virtual returns (EIP1559ScriptArtifact memory) {
  ::284 =>     function rawToConvertedEIPTx1559s(RawTx1559[] memory rawTxs) internal pure virtual returns (Tx1559[] memory) {
  ::292 =>     function rawToConvertedEIPTx1559(RawTx1559 memory rawTx) internal pure virtual returns (Tx1559 memory) {
  ::303 =>     function rawToConvertedEIP1559Detail(RawTx1559Detail memory rawDetail) internal pure virtual returns (Tx1559Detail memory) {
  ::316 =>     function readTx1559s(string memory path) internal view virtual returns (Tx1559[] memory) {
  ::323 =>     function readTx1559(string memory path, uint256 index) internal view virtual returns (Tx1559 memory) {
  ::332 =>     function readReceipts(string memory path) internal view virtual returns (Receipt[] memory) {
  ::339 =>     function readReceipt(string memory path, uint256 index) internal view virtual returns (Receipt memory) {
  ::347 =>     function rawToConvertedReceipts(RawReceipt[] memory rawReceipts) internal pure virtual returns (Receipt[] memory) {
  ::355 =>     function rawToConvertedReceipt(RawReceipt memory rawReceipt) internal pure virtual returns (Receipt memory) {
  ::373 =>     function rawToConvertedReceiptLogs(RawReceiptLog[] memory rawLogs) internal pure virtual returns (ReceiptLog[] memory) {
  ::450 =>     function _bytesToUint(bytes memory b) private pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdJson.sol
  ```
  ::32 =>     function parseRaw(string memory json, string memory key) internal pure returns (bytes memory) {
  ::36 =>     function readUint(string memory json, string memory key) internal pure returns (uint256) {
  ::40 =>     function readUintArray(string memory json, string memory key) internal pure returns (uint256[] memory) {
  ::44 =>     function readInt(string memory json, string memory key) internal pure returns (int256) {
  ::48 =>     function readIntArray(string memory json, string memory key) internal pure returns (int256[] memory) {
  ::52 =>     function readBytes32(string memory json, string memory key) internal pure returns (bytes32) {
  ::56 =>     function readBytes32Array(string memory json, string memory key) internal pure returns (bytes32[] memory) {
  ::60 =>     function readString(string memory json, string memory key) internal pure returns (string memory) {
  ::64 =>     function readStringArray(string memory json, string memory key) internal pure returns (string[] memory) {
  ::68 =>     function readAddress(string memory json, string memory key) internal pure returns (address) {
  ::72 =>     function readAddressArray(string memory json, string memory key) internal pure returns (address[] memory) {
  ::76 =>     function readBool(string memory json, string memory key) internal pure returns (bool) {
  ::80 =>     function readBoolArray(string memory json, string memory key) internal pure returns (bool[] memory) {
  ::84 =>     function readBytes(string memory json, string memory key) internal pure returns (bytes memory) {
  ::88 =>     function readBytesArray(string memory json, string memory key) internal pure returns (bytes[] memory) {
  ::92 =>     function serialize(string memory jsonKey, string memory key, bool value) internal returns (string memory) {
  ::96 =>     function serialize(string memory jsonKey, string memory key, bool[] memory value) internal returns (string memory) {
  ::100 =>     function serialize(string memory jsonKey, string memory key, uint256 value) internal returns (string memory) {
  ::104 =>     function serialize(string memory jsonKey, string memory key, uint256[] memory value) internal returns (string memory) {
  ::108 =>     function serialize(string memory jsonKey, string memory key, int256 value) internal returns (string memory) {
  ::112 =>     function serialize(string memory jsonKey, string memory key, int256[] memory value) internal returns (string memory) {
  ::116 =>     function serialize(string memory jsonKey, string memory key, address value) internal returns (string memory) {
  ::120 =>     function serialize(string memory jsonKey, string memory key, address[] memory value) internal returns (string memory) {
  ::124 =>     function serialize(string memory jsonKey, string memory key, bytes32 value) internal returns (string memory) {
  ::128 =>     function serialize(string memory jsonKey, string memory key, bytes32[] memory value) internal returns (string memory) {
  ::132 =>     function serialize(string memory jsonKey, string memory key, bytes memory value) internal returns (string memory) {
  ::136 =>     function serialize(string memory jsonKey, string memory key, bytes[] memory value) internal returns (string memory) {
  ::140 =>     function serialize(string memory jsonKey, string memory key, string memory value) internal returns (string memory) {
  ::144 =>     function serialize(string memory jsonKey, string memory key, string[] memory value) internal returns (string memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdMath.sol
  ```
  ::7 =>     function abs(int256 a) internal pure returns (uint256) {
  ::16 =>     function delta(uint256 a, uint256 b) internal pure returns (uint256) {
  ::20 =>     function delta(int256 a, int256 b) internal pure returns (uint256) {
  ::31 =>     function percentDelta(uint256 a, uint256 b) internal pure returns (uint256) {
  ::37 =>     function percentDelta(int256 a, int256 b) internal pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdStorage.sol
  ```
  ::22 =>     function sigs(string memory sigStr) internal pure returns (bytes4) {
  ::32 =>     function find(StdStorage storage self) internal returns (uint256) {
  ::101 =>     function target(StdStorage storage self, address _target) internal returns (StdStorage storage) {
  ::106 =>     function sig(StdStorage storage self, bytes4 _sig) internal returns (StdStorage storage) {
  ::111 =>     function sig(StdStorage storage self, string memory _sig) internal returns (StdStorage storage) {
  ::116 =>     function with_key(StdStorage storage self, address who) internal returns (StdStorage storage) {
  ::121 =>     function with_key(StdStorage storage self, uint256 amt) internal returns (StdStorage storage) {
  ::126 =>     function with_key(StdStorage storage self, bytes32 key) internal returns (StdStorage storage) {
  ::131 =>     function depth(StdStorage storage self, uint256 _depth) internal returns (StdStorage storage) {
  ::136 =>     function read(StdStorage storage self) private returns (bytes memory) {
  ::142 =>     function read_bytes32(StdStorage storage self) internal returns (bytes32) {
  ::146 =>     function read_bool(StdStorage storage self) internal returns (bool) {
  ::153 =>     function read_address(StdStorage storage self) internal returns (address) {
  ::157 =>     function read_uint(StdStorage storage self) internal returns (uint256) {
  ::161 =>     function read_int(StdStorage storage self) internal returns (int256) {
  ::165 =>     function bytesToBytes32(bytes memory b, uint256 offset) private pure returns (bytes32) {
  ::175 =>     function flatten(bytes32[] memory b) private pure returns (bytes memory) {
  ::192 =>     function sigs(string memory sigStr) internal pure returns (bytes4) {
  ::196 =>     function find(StdStorage storage self) internal returns (uint256) {
  ::200 =>     function target(StdStorage storage self, address _target) internal returns (StdStorage storage) {
  ::204 =>     function sig(StdStorage storage self, bytes4 _sig) internal returns (StdStorage storage) {
  ::208 =>     function sig(StdStorage storage self, string memory _sig) internal returns (StdStorage storage) {
  ::212 =>     function with_key(StdStorage storage self, address who) internal returns (StdStorage storage) {
  ::216 =>     function with_key(StdStorage storage self, uint256 amt) internal returns (StdStorage storage) {
  ::220 =>     function with_key(StdStorage storage self, bytes32 key) internal returns (StdStorage storage) {
  ::224 =>     function depth(StdStorage storage self, uint256 _depth) internal returns (StdStorage storage) {
  ::274 =>     function read_bytes32(StdStorage storage self) internal returns (bytes32) {
  ::278 =>     function read_bool(StdStorage storage self) internal returns (bool) {
  ::282 =>     function read_address(StdStorage storage self) internal returns (address) {
  ::286 =>     function read_uint(StdStorage storage self) internal returns (uint256) {
  ::290 =>     function read_int(StdStorage storage self) internal returns (int256) {
  ::295 =>     function bytesToBytes32(bytes memory b, uint256 offset) private pure returns (bytes32) {
  ::306 =>     function flatten(bytes32[] memory b) private pure returns (bytes memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/src/StdUtils.sol
  ```
  ::44 =>     function computeCreateAddress(address deployer, uint256 nonce) internal pure virtual returns (address) {
  ::65 =>     function computeCreate2Address(bytes32 salt, bytes32 initcodeHash, address deployer) internal pure virtual returns (address) {
  ::69 =>     function bytesToUint(bytes memory b) internal pure virtual returns (uint256) {
  ::74 =>     function addressFromLast20Bytes(bytes32 bytesValue) private pure returns (address) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol
  ```
  ::12 =>     function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol
  ```
  ::12 =>     function catchError(bytes memory reason, bytes4 selector) internal pure returns (bytes memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol
  ```
  ::16 =>     function convert(uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) internal pure returns (uint256) {
  ::26 =>     function turn(uint256 amount, uint256 strike, bool toOne, bool roundUp) internal pure returns (uint256) {
  ::35 =>     function combine(uint256 amount0, uint256 amount1, uint256 strike, bool roundUp) internal pure returns (uint256) {
  ::46 =>     function dif(uint256 base, uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) internal pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/BytesLibExt.sol
  ```
  ::7 =>     function slice(bytes memory self, uint start, uint len) external pure returns (bytes memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/CatchErrorExt.sol
  ```
  ::6 =>     function catchError(bytes memory reason, bytes4 selector) external pure returns (bytes memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/test/wrapped/StrikeConversionExt.sol
  ```
  ::6 =>     function convert(uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) external pure returns (uint256) {
  ::10 =>     function turn(uint256 amount, uint256 strike, bool toOne, bool roundUp) external pure returns (uint256) {
  ::14 =>     function combine(uint256 amount0, uint256 amount1, uint256 strike, bool roundUp) external pure returns (uint256) {
  ::18 =>     function dif(uint256 base, uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) external pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol
  ```
  ::70 =>     function blockTimestamp() internal view virtual returns (uint96) {
  ::76 =>     function getByIndex(uint256 id) external view override returns (StrikeAndMaturity memory) {
  ::80 =>     function numberOfOptions() external view override returns (uint256) {
  ::85 =>     function totalPosition(uint256 strike, uint256 maturity, TimeswapV2OptionPosition position) external view override returns (uint256) {
  ::90 =>     function positionOf(uint256 strike, uint256 maturity, address owner, TimeswapV2OptionPosition position) external view override returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol
  ```
  ::36 =>     function numberOfPairs() external view override returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/Proportion.sol
  ```
  ::12 =>     function proportion(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) internal pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/OptionFactoryExt.sol
  ```
  ::11 =>     function get(address optionFactory, address token0, address token1) public view returns (address) {
  ::15 =>     function getWithCheck(address optionFactory, address token0, address token1) public view returns (address) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/test/wrappedContracts/ProportionExt.sol
  ```
  ::7 =>     function proportion(uint256 multiplicand, uint256 multiplier, uint256 divisor, bool roundUp) public pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/lib/ds-test/demo/demo.sol
  ```
  ::62 =>     function echo(string memory s1, string memory s2) public pure returns (string memory, string memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/lib/ds-test/src/test.sol
  ```
  ::50 =>     function failed() public returns (bool) {
  ::71 =>     function hasHEVMContext() internal view returns (bool) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdChains.sol
  ```
  ::107 =>     function getChainWithUpdatedRpcUrl(string memory chainAlias, Chain memory chain) private view returns (Chain memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdCheats.sol
  ```
  ::223 =>     function readEIP1559ScriptArtifact(string memory path) internal view virtual returns (EIP1559ScriptArtifact memory) {
  ::238 =>     function rawToConvertedEIPTx1559s(RawTx1559[] memory rawTxs) internal pure virtual returns (Tx1559[] memory) {
  ::246 =>     function rawToConvertedEIPTx1559(RawTx1559 memory rawTx) internal pure virtual returns (Tx1559 memory) {
  ::257 =>     function rawToConvertedEIP1559Detail(RawTx1559Detail memory rawDetail) internal pure virtual returns (Tx1559Detail memory) {
  ::270 =>     function readTx1559s(string memory path) internal view virtual returns (Tx1559[] memory) {
  ::277 =>     function readTx1559(string memory path, uint256 index) internal view virtual returns (Tx1559 memory) {
  ::286 =>     function readReceipts(string memory path) internal view virtual returns (Receipt[] memory) {
  ::293 =>     function readReceipt(string memory path, uint256 index) internal view virtual returns (Receipt memory) {
  ::301 =>     function rawToConvertedReceipts(RawReceipt[] memory rawReceipts) internal pure virtual returns (Receipt[] memory) {
  ::309 =>     function rawToConvertedReceipt(RawReceipt memory rawReceipt) internal pure virtual returns (Receipt memory) {
  ::327 =>     function rawToConvertedReceiptLogs(RawReceiptLog[] memory rawLogs) internal pure virtual returns (ReceiptLog[] memory) {
  ::404 =>     function _bytesToUint(bytes memory b) private pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdJson.sol
  ```
  ::32 =>     function parseRaw(string memory json, string memory key) internal pure returns (bytes memory) {
  ::36 =>     function readUint(string memory json, string memory key) internal pure returns (uint256) {
  ::40 =>     function readUintArray(string memory json, string memory key) internal pure returns (uint256[] memory) {
  ::44 =>     function readInt(string memory json, string memory key) internal pure returns (int256) {
  ::48 =>     function readIntArray(string memory json, string memory key) internal pure returns (int256[] memory) {
  ::52 =>     function readBytes32(string memory json, string memory key) internal pure returns (bytes32) {
  ::56 =>     function readBytes32Array(string memory json, string memory key) internal pure returns (bytes32[] memory) {
  ::60 =>     function readString(string memory json, string memory key) internal pure returns (string memory) {
  ::64 =>     function readStringArray(string memory json, string memory key) internal pure returns (string[] memory) {
  ::68 =>     function readAddress(string memory json, string memory key) internal pure returns (address) {
  ::72 =>     function readAddressArray(string memory json, string memory key) internal pure returns (address[] memory) {
  ::76 =>     function readBool(string memory json, string memory key) internal pure returns (bool) {
  ::80 =>     function readBoolArray(string memory json, string memory key) internal pure returns (bool[] memory) {
  ::84 =>     function readBytes(string memory json, string memory key) internal pure returns (bytes memory) {
  ::88 =>     function readBytesArray(string memory json, string memory key) internal pure returns (bytes[] memory) {
  ::92 =>     function serialize(string memory jsonKey, string memory key, bool value) internal returns (string memory) {
  ::96 =>     function serialize(string memory jsonKey, string memory key, bool[] memory value) internal returns (string memory) {
  ::100 =>     function serialize(string memory jsonKey, string memory key, uint256 value) internal returns (string memory) {
  ::104 =>     function serialize(string memory jsonKey, string memory key, uint256[] memory value) internal returns (string memory) {
  ::108 =>     function serialize(string memory jsonKey, string memory key, int256 value) internal returns (string memory) {
  ::112 =>     function serialize(string memory jsonKey, string memory key, int256[] memory value) internal returns (string memory) {
  ::116 =>     function serialize(string memory jsonKey, string memory key, address value) internal returns (string memory) {
  ::120 =>     function serialize(string memory jsonKey, string memory key, address[] memory value) internal returns (string memory) {
  ::124 =>     function serialize(string memory jsonKey, string memory key, bytes32 value) internal returns (string memory) {
  ::128 =>     function serialize(string memory jsonKey, string memory key, bytes32[] memory value) internal returns (string memory) {
  ::132 =>     function serialize(string memory jsonKey, string memory key, bytes memory value) internal returns (string memory) {
  ::136 =>     function serialize(string memory jsonKey, string memory key, bytes[] memory value) internal returns (string memory) {
  ::140 =>     function serialize(string memory jsonKey, string memory key, string memory value) internal returns (string memory) {
  ::144 =>     function serialize(string memory jsonKey, string memory key, string[] memory value) internal returns (string memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdMath.sol
  ```
  ::7 =>     function abs(int256 a) internal pure returns (uint256) {
  ::16 =>     function delta(uint256 a, uint256 b) internal pure returns (uint256) {
  ::20 =>     function delta(int256 a, int256 b) internal pure returns (uint256) {
  ::31 =>     function percentDelta(uint256 a, uint256 b) internal pure returns (uint256) {
  ::37 =>     function percentDelta(int256 a, int256 b) internal pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdStorage.sol
  ```
  ::22 =>     function sigs(string memory sigStr) internal pure returns (bytes4) {
  ::32 =>     function find(StdStorage storage self) internal returns (uint256) {
  ::101 =>     function target(StdStorage storage self, address _target) internal returns (StdStorage storage) {
  ::106 =>     function sig(StdStorage storage self, bytes4 _sig) internal returns (StdStorage storage) {
  ::111 =>     function sig(StdStorage storage self, string memory _sig) internal returns (StdStorage storage) {
  ::116 =>     function with_key(StdStorage storage self, address who) internal returns (StdStorage storage) {
  ::121 =>     function with_key(StdStorage storage self, uint256 amt) internal returns (StdStorage storage) {
  ::126 =>     function with_key(StdStorage storage self, bytes32 key) internal returns (StdStorage storage) {
  ::131 =>     function depth(StdStorage storage self, uint256 _depth) internal returns (StdStorage storage) {
  ::136 =>     function read(StdStorage storage self) private returns (bytes memory) {
  ::142 =>     function read_bytes32(StdStorage storage self) internal returns (bytes32) {
  ::146 =>     function read_bool(StdStorage storage self) internal returns (bool) {
  ::153 =>     function read_address(StdStorage storage self) internal returns (address) {
  ::157 =>     function read_uint(StdStorage storage self) internal returns (uint256) {
  ::161 =>     function read_int(StdStorage storage self) internal returns (int256) {
  ::165 =>     function bytesToBytes32(bytes memory b, uint256 offset) private pure returns (bytes32) {
  ::175 =>     function flatten(bytes32[] memory b) private pure returns (bytes memory) {
  ::192 =>     function sigs(string memory sigStr) internal pure returns (bytes4) {
  ::196 =>     function find(StdStorage storage self) internal returns (uint256) {
  ::200 =>     function target(StdStorage storage self, address _target) internal returns (StdStorage storage) {
  ::204 =>     function sig(StdStorage storage self, bytes4 _sig) internal returns (StdStorage storage) {
  ::208 =>     function sig(StdStorage storage self, string memory _sig) internal returns (StdStorage storage) {
  ::212 =>     function with_key(StdStorage storage self, address who) internal returns (StdStorage storage) {
  ::216 =>     function with_key(StdStorage storage self, uint256 amt) internal returns (StdStorage storage) {
  ::220 =>     function with_key(StdStorage storage self, bytes32 key) internal returns (StdStorage storage) {
  ::224 =>     function depth(StdStorage storage self, uint256 _depth) internal returns (StdStorage storage) {
  ::274 =>     function read_bytes32(StdStorage storage self) internal returns (bytes32) {
  ::278 =>     function read_bool(StdStorage storage self) internal returns (bool) {
  ::282 =>     function read_address(StdStorage storage self) internal returns (address) {
  ::286 =>     function read_uint(StdStorage storage self) internal returns (uint256) {
  ::290 =>     function read_int(StdStorage storage self) internal returns (int256) {
  ::295 =>     function bytesToBytes32(bytes memory b, uint256 offset) private pure returns (bytes32) {
  ::306 =>     function flatten(bytes32[] memory b) private pure returns (bytes memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/lib/forge-std/src/StdUtils.sol
  ```
  ::69 =>     function computeCreateAddress(address deployer, uint256 nonce) internal pure virtual returns (address) {
  ::90 =>     function computeCreate2Address(bytes32 salt, bytes32 initcodeHash, address deployer) internal pure virtual returns (address) {
  ::94 =>     function bytesToUint(bytes memory b) internal pure virtual returns (uint256) {
  ::99 =>     function addressFromLast20Bytes(bytes32 bytesValue) private pure returns (address) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol
  ```
  ::82 =>     function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {
  ::89 =>     function getByIndex(uint256 id) external view override returns (StrikeAndMaturity memory) {
  ::94 =>     function numberOfPools() external view override returns (uint256) {
  ::103 =>     function totalLiquidity(uint256 strike, uint256 maturity) external view override returns (uint160) {
  ::108 =>     function sqrtInterestRate(uint256 strike, uint256 maturity) external view override returns (uint160) {
  ::113 =>     function liquidityOf(uint256 strike, uint256 maturity, address owner) external view override returns (uint160) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol
  ```
  ::29 =>     function getLong(uint160 liquidity, uint160 rate, bool roundUp) internal pure returns (uint256) {
  ::38 =>     function getShort(uint160 liquidity, uint160 rate, uint96 duration, bool roundUp) internal pure returns (uint256) {
  ::259 =>     function getLiquidityGivenLong(uint160 rate, uint256 longAmount, bool roundUp) private pure returns (uint160) {
  ::268 =>     function getLiquidityGivenShort(uint160 rate, uint256 shortAmount, uint96 duration, bool roundUp) private pure returns (uint160) {
  ::277 =>     function getNewSqrtInterestRateGivenLong(uint160 liquidity, uint160 rate, uint256 longAmount, bool isAdd) private pure returns (uint160) {
  ::329 =>     function getLongFromSqrtInterestRate(uint160 liquidity, uint160 rate, uint160 deltaRate, uint160 newRate, bool roundUp) private pure returns (uint256) {
  ::342 =>     function getShortFromSqrtInterestRate(uint160 liquidity, uint160 deltaRate, uint96 duration, bool roundUp) private pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/Duration.sol
  ```
  ::11 =>     function init(uint256 duration) internal pure returns (uint96) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol
  ```
  ::40 =>     function getFees(uint160 liquidity, uint256 lastFeeGrowth, uint256 globalFeeGrowth) internal pure returns (uint256) {
  ::47 =>     function addFees(uint256 amount, uint256 fee) internal pure returns (uint256) {
  ::54 =>     function removeFees(uint256 amount, uint256 fee) internal pure returns (uint256) {
  ::61 =>     function getFeesRemoval(uint256 amount, uint256 fee) internal pure returns (uint256) {
  ::68 =>     function getFeesAdditional(uint256 amount, uint256 fee) internal pure returns (uint256) {
  ::75 =>     function getFeeGrowth(uint256 feeAmount, uint160 liquidity) internal pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/ConstantProductExt.sol
  ```
  ::7 =>     function getLong(uint160 liquidity, uint160 rate, bool roundUp) public pure returns (uint256) {
  ::11 =>     function getShort(uint160 liquidity, uint160 rate, uint96 duration, bool roundUp) public pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationCalculationExt.sol
  ```
  ::7 =>     function unsafeDurationFromLastTimestampToNow(uint96 lastTimestamp, uint96 blockTimestamp) public pure returns (uint96) {
  ::11 =>     function unsafeDurationFromNowToMaturity(uint256 maturity, uint96 blockTimestamp) public pure returns (uint96) {
  ::15 =>     function unsafeDurationFromLastTimestampToMaturity(uint96 lastTimestamp, uint256 maturity) public pure returns (uint96) {
  ::19 =>     function unsafeDurationFromLastTimestampToNowOrMaturity(uint96 lastTimestamp, uint256 maturity, uint96 blockTimestamp) public pure returns (uint96) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationExt.sol
  ```
  ::7 =>     function init(uint256 duration) public pure returns (uint96) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/test/wrappedContracts/DurationWeightExt.sol
  ```
  ::7 =>     function update(uint160 liquidity, uint256 shortFeeGrowth, uint256 shortAmount) public pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/lib/ds-test/demo/demo.sol
  ```
  ::62 =>     function echo(string memory s1, string memory s2) public pure returns (string memory, string memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/lib/ds-test/src/test.sol
  ```
  ::50 =>     function failed() public returns (bool) {
  ::71 =>     function hasHEVMContext() internal view returns (bool) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdChains.sol
  ```
  ::71 =>     function getChain(string memory chainAlias) internal virtual returns (Chain memory chain) {
  ::81 =>     function getChain(uint256 chainId) internal virtual returns (Chain memory chain) {
  ::119 =>     function _toUpper(string memory str) private pure returns (string memory) {
  ::135 =>     function getChainWithUpdatedRpcUrl(string memory chainAlias, Chain memory chain) private returns (Chain memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdCheats.sol
  ```
  ::223 =>     function readEIP1559ScriptArtifact(string memory path) internal view virtual returns (EIP1559ScriptArtifact memory) {
  ::238 =>     function rawToConvertedEIPTx1559s(RawTx1559[] memory rawTxs) internal pure virtual returns (Tx1559[] memory) {
  ::246 =>     function rawToConvertedEIPTx1559(RawTx1559 memory rawTx) internal pure virtual returns (Tx1559 memory) {
  ::257 =>     function rawToConvertedEIP1559Detail(RawTx1559Detail memory rawDetail) internal pure virtual returns (Tx1559Detail memory) {
  ::270 =>     function readTx1559s(string memory path) internal view virtual returns (Tx1559[] memory) {
  ::277 =>     function readTx1559(string memory path, uint256 index) internal view virtual returns (Tx1559 memory) {
  ::286 =>     function readReceipts(string memory path) internal view virtual returns (Receipt[] memory) {
  ::293 =>     function readReceipt(string memory path, uint256 index) internal view virtual returns (Receipt memory) {
  ::301 =>     function rawToConvertedReceipts(RawReceipt[] memory rawReceipts) internal pure virtual returns (Receipt[] memory) {
  ::309 =>     function rawToConvertedReceipt(RawReceipt memory rawReceipt) internal pure virtual returns (Receipt memory) {
  ::327 =>     function rawToConvertedReceiptLogs(RawReceiptLog[] memory rawLogs) internal pure virtual returns (ReceiptLog[] memory) {
  ::404 =>     function _bytesToUint(bytes memory b) private pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdJson.sol
  ```
  ::32 =>     function parseRaw(string memory json, string memory key) internal pure returns (bytes memory) {
  ::36 =>     function readUint(string memory json, string memory key) internal pure returns (uint256) {
  ::40 =>     function readUintArray(string memory json, string memory key) internal pure returns (uint256[] memory) {
  ::44 =>     function readInt(string memory json, string memory key) internal pure returns (int256) {
  ::48 =>     function readIntArray(string memory json, string memory key) internal pure returns (int256[] memory) {
  ::52 =>     function readBytes32(string memory json, string memory key) internal pure returns (bytes32) {
  ::56 =>     function readBytes32Array(string memory json, string memory key) internal pure returns (bytes32[] memory) {
  ::60 =>     function readString(string memory json, string memory key) internal pure returns (string memory) {
  ::64 =>     function readStringArray(string memory json, string memory key) internal pure returns (string[] memory) {
  ::68 =>     function readAddress(string memory json, string memory key) internal pure returns (address) {
  ::72 =>     function readAddressArray(string memory json, string memory key) internal pure returns (address[] memory) {
  ::76 =>     function readBool(string memory json, string memory key) internal pure returns (bool) {
  ::80 =>     function readBoolArray(string memory json, string memory key) internal pure returns (bool[] memory) {
  ::84 =>     function readBytes(string memory json, string memory key) internal pure returns (bytes memory) {
  ::88 =>     function readBytesArray(string memory json, string memory key) internal pure returns (bytes[] memory) {
  ::92 =>     function serialize(string memory jsonKey, string memory key, bool value) internal returns (string memory) {
  ::96 =>     function serialize(string memory jsonKey, string memory key, bool[] memory value) internal returns (string memory) {
  ::100 =>     function serialize(string memory jsonKey, string memory key, uint256 value) internal returns (string memory) {
  ::104 =>     function serialize(string memory jsonKey, string memory key, uint256[] memory value) internal returns (string memory) {
  ::108 =>     function serialize(string memory jsonKey, string memory key, int256 value) internal returns (string memory) {
  ::112 =>     function serialize(string memory jsonKey, string memory key, int256[] memory value) internal returns (string memory) {
  ::116 =>     function serialize(string memory jsonKey, string memory key, address value) internal returns (string memory) {
  ::120 =>     function serialize(string memory jsonKey, string memory key, address[] memory value) internal returns (string memory) {
  ::124 =>     function serialize(string memory jsonKey, string memory key, bytes32 value) internal returns (string memory) {
  ::128 =>     function serialize(string memory jsonKey, string memory key, bytes32[] memory value) internal returns (string memory) {
  ::132 =>     function serialize(string memory jsonKey, string memory key, bytes memory value) internal returns (string memory) {
  ::136 =>     function serialize(string memory jsonKey, string memory key, bytes[] memory value) internal returns (string memory) {
  ::140 =>     function serialize(string memory jsonKey, string memory key, string memory value) internal returns (string memory) {
  ::144 =>     function serialize(string memory jsonKey, string memory key, string[] memory value) internal returns (string memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdMath.sol
  ```
  ::7 =>     function abs(int256 a) internal pure returns (uint256) {
  ::16 =>     function delta(uint256 a, uint256 b) internal pure returns (uint256) {
  ::20 =>     function delta(int256 a, int256 b) internal pure returns (uint256) {
  ::31 =>     function percentDelta(uint256 a, uint256 b) internal pure returns (uint256) {
  ::37 =>     function percentDelta(int256 a, int256 b) internal pure returns (uint256) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdStorage.sol
  ```
  ::22 =>     function sigs(string memory sigStr) internal pure returns (bytes4) {
  ::32 =>     function find(StdStorage storage self) internal returns (uint256) {
  ::101 =>     function target(StdStorage storage self, address _target) internal returns (StdStorage storage) {
  ::106 =>     function sig(StdStorage storage self, bytes4 _sig) internal returns (StdStorage storage) {
  ::111 =>     function sig(StdStorage storage self, string memory _sig) internal returns (StdStorage storage) {
  ::116 =>     function with_key(StdStorage storage self, address who) internal returns (StdStorage storage) {
  ::121 =>     function with_key(StdStorage storage self, uint256 amt) internal returns (StdStorage storage) {
  ::126 =>     function with_key(StdStorage storage self, bytes32 key) internal returns (StdStorage storage) {
  ::131 =>     function depth(StdStorage storage self, uint256 _depth) internal returns (StdStorage storage) {
  ::136 =>     function read(StdStorage storage self) private returns (bytes memory) {
  ::142 =>     function read_bytes32(StdStorage storage self) internal returns (bytes32) {
  ::146 =>     function read_bool(StdStorage storage self) internal returns (bool) {
  ::153 =>     function read_address(StdStorage storage self) internal returns (address) {
  ::157 =>     function read_uint(StdStorage storage self) internal returns (uint256) {
  ::161 =>     function read_int(StdStorage storage self) internal returns (int256) {
  ::165 =>     function bytesToBytes32(bytes memory b, uint256 offset) private pure returns (bytes32) {
  ::175 =>     function flatten(bytes32[] memory b) private pure returns (bytes memory) {
  ::192 =>     function sigs(string memory sigStr) internal pure returns (bytes4) {
  ::196 =>     function find(StdStorage storage self) internal returns (uint256) {
  ::200 =>     function target(StdStorage storage self, address _target) internal returns (StdStorage storage) {
  ::204 =>     function sig(StdStorage storage self, bytes4 _sig) internal returns (StdStorage storage) {
  ::208 =>     function sig(StdStorage storage self, string memory _sig) internal returns (StdStorage storage) {
  ::212 =>     function with_key(StdStorage storage self, address who) internal returns (StdStorage storage) {
  ::216 =>     function with_key(StdStorage storage self, uint256 amt) internal returns (StdStorage storage) {
  ::220 =>     function with_key(StdStorage storage self, bytes32 key) internal returns (StdStorage storage) {
  ::224 =>     function depth(StdStorage storage self, uint256 _depth) internal returns (StdStorage storage) {
  ::274 =>     function read_bytes32(StdStorage storage self) internal returns (bytes32) {
  ::278 =>     function read_bool(StdStorage storage self) internal returns (bool) {
  ::282 =>     function read_address(StdStorage storage self) internal returns (address) {
  ::286 =>     function read_uint(StdStorage storage self) internal returns (uint256) {
  ::290 =>     function read_int(StdStorage storage self) internal returns (int256) {
  ::295 =>     function bytesToBytes32(bytes memory b, uint256 offset) private pure returns (bytes32) {
  ::306 =>     function flatten(bytes32[] memory b) private pure returns (bytes memory) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/lib/forge-std/src/StdUtils.sol
  ```
  ::78 =>     function bytesToUint(bytes memory b) internal pure virtual returns (uint256) {
  ::85 =>     function computeCreateAddress(address deployer, uint256 nonce) internal pure virtual returns (address) {
  ::106 =>     function computeCreate2Address(bytes32 salt, bytes32 initcodeHash, address deployer) internal pure virtual returns (address) {
  ::111 =>     function getTokenBalances(address token, address[] memory addresses) internal virtual returns (uint256[] memory balances) {
  ::140 =>     function addressFromLast20Bytes(bytes32 bytesValue) private pure returns (address) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol
  ```
  ::99 =>     function mint(TimeswapV2LiquidityTokenMintParam calldata param) external returns (bytes memory data) {
  ::150 =>     function burn(TimeswapV2LiquidityTokenBurnParam calldata param) external returns (bytes memory data) {
  ::255 =>     function _additionalConditionAddTokenToOwnerEnumeration(address to, uint256 id) internal view override returns (bool) {
  ::259 =>     function _additionalConditionRemoveTokenFromOwnerEnumeration(address from, uint256 id) internal view override returns (bool) {
  ::263 =>     function _additionalConditionForOwnerTokenEnumeration(address owner, uint256 id) private view returns (bool) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol
  ```
  ::31 =>     function tokenOfOwnerByIndex(address owner, uint256 index) external view override returns (uint256) {
  ::36 =>     function totalSupply() public view override returns (uint256) {
  ::41 =>     function tokenByIndex(uint256 index) external view override returns (uint256) {
  ::70 =>     function _additionalConditionAddTokenToAllTokensEnumeration(uint256) internal virtual returns (bool) {
  ::75 =>     function _additionalConditionAddTokenToOwnerEnumeration(address, uint256) internal virtual returns (bool) {
  ::104 =>     function _additionalConditionRemoveTokenFromAllTokensEnumeration(uint256) internal virtual returns (bool) {
  ::109 =>     function _additionalConditionRemoveTokenFromOwnerEnumeration(address, uint256) internal virtual returns (bool) {
  ```
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol
  ```
  ::34 =>     function toKey(TimeswapV2TokenPosition memory timeswapV2TokenPosition) internal pure returns (bytes32) {
  ::39 =>     function toKey(TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition) internal pure returns (bytes32) {
  ```

### References

- [Unnamed return parameters | Opyn Bull Strategy Contracts Audit](https://blog.openzeppelin.com/opyn-bull-strategy-contracts-audit/#unnamed-return-parameters)