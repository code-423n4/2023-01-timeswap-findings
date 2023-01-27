## QA Report

| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | USE OF FLOATING PRAGMA | 8 |
| [L-2](#L-2) | DUMMY URI USED IN `TimeswapV2Token` AND `TimeswapV2LiquidityToken` | 2 |
| [L-3](#L-3) | NO ZERO ADDRESS CHECK | 1 |
| [NC-1](#NC-1) | INCORRECT NATSPEC | 1 |
| [NC-2](#NC-2) | SPELLING ERROR IN NATSPEC | 76 |
| [NC-3](#NC-3) | INCORRECT ERROR TYPE | 1 |
| [NC-4](#NC-4) | 4 METHODS WITH `durationForward` AS SECOND PARAMETER CAN BE REMOVED | 4 |

###  [L-1] USE OF FLOATING PRAGMA

Impact: [swcregistry](https://swcregistry.io/docs/SWC-103)

*Instances (8):*
```solidity
File: v2-token/src/TimeswapV2LiquidityToken.sol

2:    pragma solidity ^0.8.8;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L2)

```solidity
File: v2-token/src/TimeswapV2Token.sol

2:    pragma solidity ^0.8.8;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L2)

```solidity
File: v2-token/src/base/ERC1155Enumerable.sol

4:    pragma solidity ^0.8.8;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L4)

```solidity
File: v2-token/src/interfaces/IERC1155Enumerable.sol

4:    pragma solidity ^0.8.8;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol#L4)

```solidity
File: v2-token/src/structs/CallbackParam.sol

2:    pragma solidity ^0.8.8;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L2)

```solidity
File: v2-token/src/structs/FeesPosition.sol

2:    pragma solidity ^0.8.8;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L2)

```solidity
File: v2-token/src/structs/Param.sol

2:    pragma solidity ^0.8.8;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol#L2)

```solidity
File: v2-token/src/structs/Position.sol

2:    pragma solidity ^0.8.8;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol#L2)

### [L-2] DUMMY URI USED IN `TimeswapV2Token` AND `TimeswapV2LiquidityToken`

Instead of setting the Real URI, Dummy URI is used.

*Instance (2):*
```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

41:      constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L41)

```solidity
File: packages/v2-token/src/TimeswapV2LiquidityToken.sol

36:      constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36)

### [L-3] NO ZERO ADDRESS CHECK

*Instance (1):*
```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

42:      optionFactory = chosenOptionFactory;

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L42)


### [NC-1] INCORRECT NATSPEC

Replace `IsSubToken0 if false` to `IsSubToken1 if false`.

*Instance (1):*
```solidity
File: packages/v2-option/src/structs/Process.sol

32:      /// @param isAddToken1 IsAddToken1 if true. IsSubToken0 if false.

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L32)

### [NC-2] SPELLING ERROR IN NATSPEC

Correct the spelling of "multiple".

*Instances:*
```solidity
File: packages/v2-option/src/structs/Process.sol

4:      /// @dev Processing information required for interacting multple options in a single contract.

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L4)

Correct the spelling of "to".

```solidity
File: packages/v2-library/src/StrikeConversion.sol

22:     /// @param amount The amount ot be converted. Token0 amount when zeroToOne. Token1 amount when oneToZero.

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L4)

Correct the spelling of "position".

```solidity
File: packages/v2-token/src/interfaces/ITimeswapV2Token.sol

27:     /// @dev mints TimeswapV2Token as per postion and amount

32:     /// @dev burns TimeswapV2Token as per postion and amount

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol)

Spelling of `recipient` is wrongly spelled as `receipient` in entire codebase (72 Instances).

### [NC-3] INCORRECT ERROR TYPE

Zero Address Error thrown when Address is not Zero.

*Instances (1):*
```solidity
File: packages/v2-pool/src/TimeswapV2PoolFactory.sol

63:      if (pair != address(0)) Error.zeroAddress();

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63)

### [NC-4] 4 METHODS WITH durationForward AS SECOND PARAMETER CAN BE REMOVED

In `TimeswapV2Pool` Contract, `mint`, `burn`, `deleverage` and `leverage` methods are used with 2 Parameters: `param` and `durationForward`.

```solidity
File: packages/v2-pool/src/TimeswapV2Pool.sol

      function mint(
          TimeswapV2PoolMintParam calldata param,
          uint96 durationForward
      ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
          return mint(param, true, durationForward);
      }


286:  if (isQuote) revert Quote();

      function burn(
          TimeswapV2PoolBurnParam calldata param,
          uint96 durationForward
      ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
          return burn(param, true, durationForward);
      }

355:  if (isQuote) revert Quote();

      function deleverage(
          TimeswapV2PoolDeleverageParam calldata param,
          uint96 durationForward
      ) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
          return deleverage(param, true, durationForward);
      }
      
407:  if (isQuote) revert Quote();

      function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
          return leverage(param, true, durationForward);
      }

457:  if (isQuote) revert Quote();

```
[Link to Code](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol)

But because of a condition in `mint` (at Line: 289), `burn` (at Line: 355), `deleverage` (at Line: 407) and `leverage` (at Line: 457) method, the function will always revert. So it is recommended to remove these functions.