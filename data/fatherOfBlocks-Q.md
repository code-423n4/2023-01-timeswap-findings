**packages/v2-library/src/Math.sol**
- L6/7 - Two errors are created, which are never used, therefore they should be eliminated.


**packages/v2-library/src/Ownership.sol**
- L22/30/38 - The three functions have the functionality of modifying the behavior of a function, therefore the correct thing would be for them to be modifiers.

	
**packages/v2-library/src/StrikeConversion.sol**
- L16/17 - The first variable that is used is zeroToOne, therefore it should be the input that is passed first, this is intuitive. Then amount, strike and roundUp.

- L26/27 - The first variable that is used is strike, therefore it should be the input that is passed first, this is intuitive. Then toOne, amount and roundUp.

- L35/36 - The first variable that is used is strike, therefore it should be the input that is passed first, this is intuitive. Then amount0, amount1 and roundUp.

- L46/48/49/50 - The first variable that is used is strike, therefore it should be the input that is passed first, this is intuitive. Then zeroToOne, base, amount and roundUp.


**packages/v2-pool/src/TimeswapV2Pool.sol**
- L21 - ITimeswapV2PoolBurnCallback is imported, but it is never used, so it should be removed.

- L32 - TimeswapV2PoolMintChoiceCallbackParam, TimeswapV2PoolBurnChoiceCallbackParam, TimeswapV2PoolDeleverageChoiceCallbackParam and TimeswapV2PoolLeverageChoiceCallbackParam are imported, but they are never used, so they should be removed.

- L34 - TransactionLibrary is imported, but it is never used, so it should be removed.

- L231 - The first variable that is used is long0Amount, therefore it should be the input that is passed first, this is intuitive. Then strike, maturity, long0To, long1To, shortTo, long1Amount and shortAmount.

- L252/254/255 - The first variable that is used is param, therefore it should be the input that is passed first, this is intuitive. Then durationForward and isQuote.


**packages/v2-pool/src/structs/Pool.sol**
- L8 - ITimeswapV2Option is imported, but it is never used, so it should be removed.

- L27 - TimeswapV2PoolCollectParam is imported, but it is never used, so it should be removed.


**packages/v2-pool/src/libraries/ConstantProduct.sol**
- L164/172/195/203/395/404 - The first variable that is used is isAdd, therefore it should be the input that is passed first, this is intuitive. Then longAmount, transactionFee, liquidity, rate and duration.


**packages/v2-pool/src/libraries/ConstantSum.sol**
- L8 - Error is imported, but it is never used, therefore it should be removed.


**packages/v2-pool/src/libraries/DurationCalculation.sol**
- L7 - DurationLibrary is imported, but it is never used, so it should be removed.


**packages/v2-token/src/TimeswapV2Token.sol**
- L5/109/170 - There is an import of a console.log, this should be removed because it interferes with the code.


**packages/v2-option/src/structs/Option.sol**
- L5/10/11 - Multiple contracts are imported (Error, PositionLibrary and TransactionLibrary), but they are never used, therefore they should be removed.


**packages/v2-option/src/TimeswapV2Option.sol**
- L24 - TimeswapV2OptionMint and TransactionLibrary are imported, but they are never used, so they should be removed.
