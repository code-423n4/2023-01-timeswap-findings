# [N-01] Lock pragmas to specific compiler version (8 INSTANCES)

**Recommendation**:

Ethereum Smart Contract Best Practices - lock pragmas to specific compiler version.
[solidity-specific/locking-pragmas](https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/)

```
/v2-token/src/TimeswapV2LiquidityToken.sol::2 => pragma solidity ^0.8.8;
/v2-token/src/TimeswapV2Token.sol::2 => pragma solidity ^0.8.8;
/v2-token/src/base/ERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
/v2-token/src/interfaces/IERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
/v2-token/src/structs/CallbackParam.sol::2 => pragma solidity ^0.8.8;
/v2-token/src/structs/FeesPosition.sol::2 => pragma solidity ^0.8.8;
/v2-token/src/structs/Param.sol::2 => pragma solidity ^0.8.8;
/v2-token/src/structs/Position.sol::2 => pragma solidity ^0.8.8;
```
