**packages/v2-pool/src/TimeswapV2Pool.sol**
- L274/297/444/461/479/512 - A variable is created in memory, but it is only used once, therefore the variable should be removed and the operation used directly where it is needed.


**packages/v2-pool/src/structs/Pool.sol**
- L679/681/700/702 - A variable is created in memory, but it is only used once, therefore the variable should be eliminated and the operation used directly where it is needed.


**packages/v2-pool/src/libraries/ConstantProduct.sol**
- L300/301/314/316/343/344/359/361 - A variable is created in memory, but it is only used once, therefore the variable should be removed and the operation used directly where it is needed.


**packages/v2-token/src/TimeswapV2LiquidityToken.sol**
- L125/143/275/277 - A variable is created in memory, but it is only used once, therefore the variable should be eliminated and the operation used directly where it is needed.


**packages/v2-token/src/TimeswapV2Token.sol**
- L61/62/81/87/259/268 - A variable is created in memory, but it is only used once, therefore the variable should be removed and the operation used directly where it is needed.

- L109 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS


**packages/v2-option/src/TimeswapV2Option.sol**
- L115/118 - A variable is created in memory, but it is only used once, therefore the variable should be eliminated and the operation used directly where it is needed.
