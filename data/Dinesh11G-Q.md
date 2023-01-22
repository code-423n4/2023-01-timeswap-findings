### Unspecific Compiler Version Pragma

#### Impact
Issue Information: 
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

Example
🤦 Bad:

pragma solidity ^0.8.0;
🚀 Good:

pragma solidity 0.8.4;

#### Findings:
```
2023-01-timeswap/packages/v2-library/lib/forge-std/src/Common.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/Components.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/Script.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdAssertions.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdCheats.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdError.sol::3 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdJson.sol::2 => pragma solidity >=0.6.0 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdMath.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdStorage.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/StdUtils.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/Test.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/Vm.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console.sol::2 => pragma solidity >=0.4.22 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/console2.sol::2 => pragma solidity >=0.4.22 <0.9.0;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/interfaces/IERC1155.sol::1 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/interfaces/IERC165.sol::1 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/interfaces/IERC20.sol::1 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/interfaces/IERC4626.sol::1 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-library/lib/forge-std/src/interfaces/IERC721.sol::1 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/Base.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/Script.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdAssertions.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdChains.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdCheats.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdError.sol::3 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdJson.sol::2 => pragma solidity >=0.6.0 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdMath.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdStorage.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/StdUtils.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/Test.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/Vm.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console.sol::2 => pragma solidity >=0.4.22 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/console2.sol::2 => pragma solidity >=0.4.22 <0.9.0;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/interfaces/IERC1155.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/interfaces/IERC165.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/interfaces/IERC20.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/interfaces/IERC4626.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-pool/lib/forge-std/src/interfaces/IERC721.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/Base.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/InvariantTest.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/Script.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdAssertions.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdChains.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdCheats.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdError.sol::3 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdJson.sol::2 => pragma solidity >=0.6.0 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdMath.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdStorage.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/StdUtils.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/Test.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/Vm.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console.sol::2 => pragma solidity >=0.4.22 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/console2.sol::2 => pragma solidity >=0.4.22 <0.9.0;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/interfaces/IERC1155.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/interfaces/IERC165.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/interfaces/IERC20.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/interfaces/IERC4626.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/interfaces/IERC721.sol::2 => pragma solidity >=0.6.2;
2023-01-timeswap/packages/v2-token/lib/forge-std/src/interfaces/IMulticall3.sol::2 => pragma solidity >=0.6.2 <0.9.0;
2023-01-timeswap/packages/v2-token/src/TimeswapV2LiquidityToken.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap/packages/v2-token/src/TimeswapV2Token.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap/packages/v2-token/src/base/ERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap/packages/v2-token/src/interfaces/IERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap/packages/v2-token/src/structs/CallbackParam.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap/packages/v2-token/src/structs/FeesPosition.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap/packages/v2-token/src/structs/Param.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap/packages/v2-token/src/structs/Position.sol::2 => pragma solidity ^0.8.8;
```
#### Tools used
Manual VS code audit