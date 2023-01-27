## [L] One of `burn()` function in TimeswapV2Pool will always revert

In `TimeswapV2Pool.sol` there are 3 `burn()` functions, which one being the actual function, the other two are just referring to it. One of these two `burn()` function, which contains 2 parameters is basically calling the actual `burn()` function with `isQuote` to true is actually useless function.

```solidity
File: TimeswapV2Pool.sol
310:     function burn(
311:         TimeswapV2PoolBurnParam calldata param,
312:         uint96 durationForward
313:     ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
314:         return burn(param, true, durationForward);
315:     }

317:     function burn(
318:         TimeswapV2PoolBurnParam calldata param,
319:         bool isQuote,
320:         uint96 durationForward
321:     ) private noDelegateCall returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {

355:         if (isQuote) revert Quote();

361:         emit Burn(param.strike, param.maturity, msg.sender, param.long0To, param.long1To, param.shortTo, liquidityAmount, long0Amount, long1Amount, shortAmount);
362:     }
```

Looking on how the actual `burn()` function is implemented (line 317), the `isQuote` is being use to block / revert if it's true. Therefore, it doesn't make any sense to create the `burn()` (with 2 parameters) function since it will always revert `Quote`.

If the `burn()` function (with 2 parameters) is by design to do that `Quote` Revert, then it's better just implement the function with a direct revert rather than passing it to actual `burn()` function.

## [L] Make any `require` or `check/validation` to top of function before anything else

In `TimeswapV2Pool.sol:collectProtocolFees()` function, `ITimeswapV2PoolFactory(poolFactory).owner().checkIfOwner();` can be move to the top to save some gas checking.

```solidity
File: TimeswapV2Pool.sol
184:     function collectProtocolFees(TimeswapV2PoolCollectParam calldata param) external override noDelegateCall returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) {
185:         ParamLibrary.check(param);
186:         raiseGuard(param.strike, param.maturity);
187:
188:         // Can only be called by the TimeswapV2Pool factory owner.
189:         ITimeswapV2PoolFactory(poolFactory).owner().checkIfOwner();
...
198:         emit CollectProtocolFees(param.strike, param.maturity, msg.sender, param.long0To, param.long1To, param.shortTo, long0Amount, long1Amount, shortAmount);
199:     }
```

## [N] Abstraction of custom `guard` for Token

The two token contract, `TimeswapV2LiquidityToken.sol` and `TimeswapV2Token.sol` contains a custom `guard` by creating this following functions:

- `changeInteractedIfNecessary()`
- `raiseGuard()`
- `lowerGuard()`

Even though the parameters between the two token is different, but actually it can be one implementation. For example, the `changeInteractedIfNecessary` in liquidity token is using `bytes32 key` as its parameter, while the TimeswapV2Token use `address token0, address token1, uint256 strike, uint256 maturity`. Actually the TimeswapV2Token can also use `bytes32 key` since the `PositionLibrary` already include the `toKey()` for the `TimeswapV2Token`.

```solidity
File: Position.sol
32: library PositionLibrary {
33:     /// @dev return keccak for key management for Token.
34:     function toKey(TimeswapV2TokenPosition memory timeswapV2TokenPosition) internal pure returns (bytes32) {
35:         return keccak256(abi.encode(timeswapV2TokenPosition));
36:     }
37:
38:     /// @dev return keccak for key management for Liquidity Token.
39:     function toKey(TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition) internal pure returns (bytes32) {
40:         return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
41:     }
42: }
```

This similarity can be propose to create an abstract contract which will inherit those functions to the Token contracts.

## [N] Cleanup `console.log`

There is a `console.log` in the codebase which is not necessarily usable when it being deployed to mainnet. Usually the `console.log` is being used to debug while in development. so, remove the `console.log` is the best option.

## [N] Function can be refactor

In `TimeswapV2Token.sol:mint()` and `TimeswapV2Token.sol:burn()` currently three are positions contains if-block which contains similar logic. This if-block with three conditions (`long0`, `long1`, `short`). It's possible to create an internal function to make it cleaner and gas efficient.

## [N] Typo

- transferrred
- receipient
- withing

## [N] Floating pragma

https://swcregistry.io/docs/SWC-103

`TimeswapV2Token` and `TimeswapV2LiquidityToken` have floating pragma and some v2-token structs contract.

```solidity
File: TimeswapV2Token.sol
1: // SPDX-License-Identifier: MIT
2: pragma solidity ^0.8.8;
```

Recommend using fixed solidity version
