# C4 Timeswap Gas Report 

## Files analyzed
- 2023-01-timeswap\packages\v2-library\src\BytesLib.sol
- 2023-01-timeswap\packages\v2-library\src\CatchError.sol
- 2023-01-timeswap\packages\v2-library\src\Error.sol
- 2023-01-timeswap\packages\v2-library\src\FullMath.sol
- 2023-01-timeswap\packages\v2-library\src\Math.sol
- 2023-01-timeswap\packages\v2-library\src\Ownership.sol
- 2023-01-timeswap\packages\v2-library\src\SafeCast.sol
- 2023-01-timeswap\packages\v2-library\src\StrikeConversion.sol
- 2023-01-timeswap\packages\v2-option\src\NoDelegateCall.sol
- 2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol
- 2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionDeployer.sol
- 2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionFactory.sol
- 2023-01-timeswap\packages\v2-option\src\enums\Position.sol
- 2023-01-timeswap\packages\v2-option\src\enums\Transaction.sol
- 2023-01-timeswap\packages\v2-option\src\interfaces\ITimeswapV2Option.sol
- 2023-01-timeswap\packages\v2-option\src\interfaces\ITimeswapV2OptionDeployer.sol
- 2023-01-timeswap\packages\v2-option\src\interfaces\ITimeswapV2OptionFactory.sol
- 2023-01-timeswap\packages\v2-option\src\interfaces\callbacks\ITimeswapV2OptionBurnCallback.sol
- 2023-01-timeswap\packages\v2-option\src\interfaces\callbacks\ITimeswapV2OptionCollectCallback.sol
- 2023-01-timeswap\packages\v2-option\src\interfaces\callbacks\ITimeswapV2OptionMintCallback.sol
- 2023-01-timeswap\packages\v2-option\src\interfaces\callbacks\ITimeswapV2OptionSwapCallback.sol
- 2023-01-timeswap\packages\v2-option\src\libraries\OptionFactory.sol
- 2023-01-timeswap\packages\v2-option\src\libraries\OptionPair.sol
- 2023-01-timeswap\packages\v2-option\src\libraries\Proportion.sol
- 2023-01-timeswap\packages\v2-option\src\structs\CallbackParam.sol
- 2023-01-timeswap\packages\v2-option\src\structs\Option.sol
- 2023-01-timeswap\packages\v2-option\src\structs\Param.sol
- 2023-01-timeswap\packages\v2-option\src\structs\Process.sol
- 2023-01-timeswap\packages\v2-option\src\structs\StrikeAndMaturity.sol
- 2023-01-timeswap\packages\v2-pool\src\NoDelegateCall.sol
- 2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol
- 2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolDeployer.sol
- 2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolFactory.sol
- 2023-01-timeswap\packages\v2-pool\src\base\OwnableTwoSteps.sol
- 2023-01-timeswap\packages\v2-pool\src\enums\Transaction.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\IOwnableTwoSteps.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\ITimeswapV2Pool.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\ITimeswapV2PoolDeployer.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\ITimeswapV2PoolFactory.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\callbacks\ITimeswapV2PoolBurnCallback.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\callbacks\ITimeswapV2PoolDeleverageCallback.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\callbacks\ITimeswapV2PoolLeverageCallback.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\callbacks\ITimeswapV2PoolMintCallback.sol
- 2023-01-timeswap\packages\v2-pool\src\interfaces\callbacks\ITimeswapV2PoolRebalanceCallback.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\ConstantProduct.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\ConstantSum.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\Duration.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\DurationCalculation.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\DurationWeight.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\Fee.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\FeeCalculation.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\PoolFactory.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\PoolPair.sol
- 2023-01-timeswap\packages\v2-pool\src\libraries\ReentrancyGuard.sol
- 2023-01-timeswap\packages\v2-pool\src\structs\CallbackParam.sol
- 2023-01-timeswap\packages\v2-pool\src\structs\LiquidityPosition.sol
- 2023-01-timeswap\packages\v2-pool\src\structs\Param.sol
- 2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol
- 2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol
- 2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol
- 2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol
- 2023-01-timeswap\packages\v2-token\src\interfaces\IERC1155Enumerable.sol
- 2023-01-timeswap\packages\v2-token\src\interfaces\ITimeswapV2LiquidityToken.sol
- 2023-01-timeswap\packages\v2-token\src\interfaces\ITimeswapV2Token.sol
- 2023-01-timeswap\packages\v2-token\src\interfaces\callbacks\ITimeswapV2LiquidityTokenBurnCallback.sol
- 2023-01-timeswap\packages\v2-token\src\interfaces\callbacks\ITimeswapV2LiquidityTokenCollectCallback.sol
- 2023-01-timeswap\packages\v2-token\src\interfaces\callbacks\ITimeswapV2LiquidityTokenMintCallback.sol
- 2023-01-timeswap\packages\v2-token\src\interfaces\callbacks\ITimeswapV2TokenBurnCallback.sol
- 2023-01-timeswap\packages\v2-token\src\interfaces\callbacks\ITimeswapV2TokenMintCallback.sol
- 2023-01-timeswap\packages\v2-token\src\structs\CallbackParam.sol
- 2023-01-timeswap\packages\v2-token\src\structs\FeesPosition.sol
- 2023-01-timeswap\packages\v2-token\src\structs\Param.sol
- 2023-01-timeswap\packages\v2-token\src\structs\Position.sol

## Issues found

### Cache Array Length Outside of Loop

#### Impact
Issue Information: [G002]

#### Findings:
```
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::12 => function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::13 => require(_length + 31 >= _length, "slice_overflow");
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::14 => require(_bytes.length >= _start + _length, "slice_outOfBounds");
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::19 => switch iszero(_length)
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::27 => // the length of that partial word and start copying that many
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::29 => // data we don't care about, but the last `lengthmod` bytes will
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::32 => // the actual length of the slice.
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::33 => let lengthmod := and(_length, 31)
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::36 => // because when slicing multiples of 32 bytes (lengthmod == 0)
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::37 => // the following copy loop was copying the origin's length
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::39 => let mc := add(add(tempBytes, lengthmod), mul(0x20, iszero(lengthmod)))
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::40 => let end := add(mc, _length)
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::45 => let cc := add(add(add(_bytes, lengthmod), mul(0x20, iszero(lengthmod))), _start)
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::53 => mstore(tempBytes, _length)
2023-01-timeswap\packages\v2-library\src\BytesLib.sol::59 => //if we want a zero-length slice let's just return a zero-length array
2023-01-timeswap\packages\v2-library\src\CatchError.sol::13 => uint256 length = reason.length;
2023-01-timeswap\packages\v2-library\src\CatchError.sol::15 => if ((length - 4) % 32 == 0 && bytes4(reason) == selector) return BytesLib.slice(reason, 4, length - 4);
2023-01-timeswap\packages\v2-library\src\CatchError.sol::18 => revert(reason, length)
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::81 => return listOfOptions.length;
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::178 => if (param.data.length != 0)
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::265 => if (param.data.length != 0)
2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionFactory.sol::37 => return getByIndex.length;
2023-01-timeswap\packages\v2-option\src\structs\Param.sol::43 => /// @notice If data length is zero, skips the calback.
2023-01-timeswap\packages\v2-option\src\structs\Param.sol::88 => /// @notice If data length is zero, skips the calback.
2023-01-timeswap\packages\v2-option\src\structs\Process.sol::34 => for (uint256 i; i < processing.length; ) {
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::95 => return listOfPools.length;
2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolFactory.sol::53 => return getByIndex.length;
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::162 => if (param.data.length != 0)
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::201 => if (param.data.length != 0)
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::227 => for (uint256 i; i < ids.length; ) {
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::215 => if (param.data.length != 0)
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::37 => return _allTokens.length;
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::48 => for (uint256 i; i < ids.length; ) {
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::82 => for (uint256 i; i < ids.length; ) {
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::118 => uint256 length = _currentIndex[to];
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::119 => _ownedTokens[to][length] = tokenId;
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::120 => _ownedTokensIndex[tokenId] = length;
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::126 => _allTokensIndex[tokenId] = _allTokens.length;
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::155 => uint256 lastTokenIndex = _allTokens.length - 1;
```

### Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: [G006]

#### Findings:
```
2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionDeployer.sol::35 => optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolDeployer.sol::28 => poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::46 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::53 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::61 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
2023-01-timeswap\packages\v2-token\src\structs\Position.sol::33 => /// @dev return keccak for key management for Token.
2023-01-timeswap\packages\v2-token\src\structs\Position.sol::35 => return keccak256(abi.encode(timeswapV2TokenPosition));
2023-01-timeswap\packages\v2-token\src\structs\Position.sol::38 => /// @dev return keccak for key management for Liquidity Token.
2023-01-timeswap\packages\v2-token\src\structs\Position.sol::40 => return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
```

### Long Revert Strings

#### Impact
Issue Information: [G007]
#### Findings:
```
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::5 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::7 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::8 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::12 => import {ITimeswapV2Option} from "./interfaces/ITimeswapV2Option.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::13 => import {ITimeswapV2OptionDeployer} from "./interfaces/ITimeswapV2OptionDeployer.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::14 => import {ITimeswapV2OptionMintCallback} from "./interfaces/callbacks/ITimeswapV2OptionMintCallback.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::15 => import {ITimeswapV2OptionBurnCallback} from "./interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::16 => import {ITimeswapV2OptionSwapCallback} from "./interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2Option.sol::17 => import {ITimeswapV2OptionCollectCallback} from "./interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionDeployer.sol::6 => import {ITimeswapV2OptionDeployer} from "./interfaces/ITimeswapV2OptionDeployer.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionFactory.sol::4 => import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionFactory.sol::5 => import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionFactory.sol::7 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-option\src\TimeswapV2OptionFactory.sol::9 => import {ITimeswapV2OptionFactory} from "./interfaces/ITimeswapV2OptionFactory.sol";
2023-01-timeswap\packages\v2-option\src\libraries\OptionFactory.sol::4 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-option\src\libraries\OptionFactory.sol::8 => import {ITimeswapV2OptionFactory} from "../interfaces/ITimeswapV2OptionFactory.sol";
2023-01-timeswap\packages\v2-option\src\libraries\Proportion.sol::4 => import {FullMath} from "@timeswap-labs/v2-library/src/FullMath.sol";
2023-01-timeswap\packages\v2-option\src\structs\Option.sol::4 => import {StrikeConversion} from "@timeswap-labs/v2-library/src/StrikeConversion.sol";
2023-01-timeswap\packages\v2-option\src\structs\Option.sol::5 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-option\src\structs\Option.sol::6 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-option\src\structs\Param.sol::4 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::4 => import {Ownership} from "@timeswap-labs/v2-library/src/Ownership.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::6 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::8 => import {ITimeswapV2Option} from "@timeswap-labs/v2-option/src/interfaces/ITimeswapV2Option.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::10 => import {TimeswapV2OptionPosition} from "@timeswap-labs/v2-option/src/enums/Position.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::12 => import {StrikeAndMaturity} from "@timeswap-labs/v2-option/src/structs/StrikeAndMaturity.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::17 => import {ITimeswapV2PoolFactory} from "./interfaces/ITimeswapV2PoolFactory.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::18 => import {ITimeswapV2PoolDeployer} from "./interfaces/ITimeswapV2PoolDeployer.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::20 => import {ITimeswapV2PoolMintCallback} from "./interfaces/callbacks/ITimeswapV2PoolMintCallback.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::21 => import {ITimeswapV2PoolBurnCallback, ITimeswapV2PoolBurn2Callback} from "./interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::22 => import {ITimeswapV2PoolDeleverageCallback} from "./interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::23 => import {ITimeswapV2PoolLeverageCallback} from "./interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2Pool.sol::24 => import {ITimeswapV2PoolRebalanceCallback} from "./interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolDeployer.sol::6 => import {ITimeswapV2PoolDeployer} from "./interfaces/ITimeswapV2PoolDeployer.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolFactory.sol::4 => import {Ownership} from "@timeswap-labs/v2-library/src/Ownership.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolFactory.sol::6 => import {OptionPairLibrary} from "@timeswap-labs/v2-option/src/libraries/OptionPair.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolFactory.sol::8 => import {ITimeswapV2PoolFactory} from "./interfaces/ITimeswapV2PoolFactory.sol";
2023-01-timeswap\packages\v2-pool\src\TimeswapV2PoolFactory.sol::12 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-pool\src\base\OwnableTwoSteps.sol::4 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-pool\src\base\OwnableTwoSteps.sol::5 => import {Ownership} from "@timeswap-labs/v2-library/src/Ownership.sol";
2023-01-timeswap\packages\v2-pool\src\base\OwnableTwoSteps.sol::7 => import {IOwnableTwoSteps} from "../interfaces/IOwnableTwoSteps.sol";
2023-01-timeswap\packages\v2-pool\src\interfaces\ITimeswapV2Pool.sol::4 => import {StrikeAndMaturity} from "@timeswap-labs/v2-option/src/structs/StrikeAndMaturity.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\ConstantProduct.sol::4 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\ConstantProduct.sol::5 => import {FullMath} from "@timeswap-labs/v2-library/src/FullMath.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\ConstantProduct.sol::7 => import {SafeCast} from "@timeswap-labs/v2-library/src/SafeCast.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\ConstantSum.sol::4 => import {StrikeConversion} from "@timeswap-labs/v2-library/src/StrikeConversion.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\ConstantSum.sol::5 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\ConstantSum.sol::8 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\DurationCalculation.sol::4 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\DurationCalculation.sol::5 => import {SafeCast} from "@timeswap-labs/v2-library/src/SafeCast.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\DurationWeight.sol::5 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\FeeCalculation.sol::4 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\FeeCalculation.sol::5 => import {FullMath} from "@timeswap-labs/v2-library/src/FullMath.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\PoolFactory.sol::4 => import {OptionFactoryLibrary} from "@timeswap-labs/v2-option/src/libraries/OptionFactory.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\PoolFactory.sol::6 => import {ITimeswapV2PoolFactory} from "../interfaces/ITimeswapV2PoolFactory.sol";
2023-01-timeswap\packages\v2-pool\src\libraries\PoolFactory.sol::8 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-pool\src\structs\LiquidityPosition.sol::4 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Param.sol::4 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::4 => import {SafeCast} from "@timeswap-labs/v2-library/src/SafeCast.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::5 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::6 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::8 => import {ITimeswapV2Option} from "@timeswap-labs/v2-option/src/interfaces/ITimeswapV2Option.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::10 => import {StrikeConversion} from "@timeswap-labs/v2-library/src/StrikeConversion.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::12 => import {DurationCalculation} from "../libraries/DurationCalculation.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::18 => import {ITimeswapV2PoolMintCallback} from "../interfaces/callbacks/ITimeswapV2PoolMintCallback.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::19 => import {ITimeswapV2PoolBurnCallback} from "../interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::20 => import {ITimeswapV2PoolDeleverageCallback} from "../interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol";
2023-01-timeswap\packages\v2-pool\src\structs\Pool.sol::21 => import {ITimeswapV2PoolLeverageCallback} from "../interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::4 => import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::6 => import {ITimeswapV2Pool} from "@timeswap-labs/v2-pool/src/interfaces/ITimeswapV2Pool.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::8 => import {PoolFactoryLibrary} from "@timeswap-labs/v2-pool/src/libraries/PoolFactory.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::9 => import {ReentrancyGuard} from "@timeswap-labs/v2-pool/src/libraries/ReentrancyGuard.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::11 => import {ITimeswapV2LiquidityToken} from "./interfaces/ITimeswapV2LiquidityToken.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::13 => import {ITimeswapV2LiquidityTokenMintCallback} from "./interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::14 => import {ITimeswapV2LiquidityTokenBurnCallback} from "./interfaces/callbacks/ITimeswapV2LiquidityTokenBurnCallback.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::15 => import {ITimeswapV2LiquidityTokenCollectCallback} from "./interfaces/callbacks/ITimeswapV2LiquidityTokenCollectCallback.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2LiquidityToken.sol::23 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::4 => import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::7 => import {ITimeswapV2Option} from "@timeswap-labs/v2-option/src/interfaces/ITimeswapV2Option.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::9 => import {OptionFactoryLibrary} from "@timeswap-labs/v2-option/src/libraries/OptionFactory.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::10 => import {ReentrancyGuard} from "@timeswap-labs/v2-pool/src/libraries/ReentrancyGuard.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::12 => import {TimeswapV2OptionPosition} from "@timeswap-labs/v2-option/src/enums/Position.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::14 => import {ITimeswapV2Token} from "./interfaces/ITimeswapV2Token.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::16 => import {ITimeswapV2TokenMintCallback} from "./interfaces/callbacks/ITimeswapV2TokenMintCallback.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::17 => import {ITimeswapV2TokenBurnCallback} from "./interfaces/callbacks/ITimeswapV2TokenBurnCallback.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::24 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-token\src\TimeswapV2Token.sol::109 => console.log("reaches right before mint in timeswapv2Tokne::mint");
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::6 => import {ERC1155} from "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
2023-01-timeswap\packages\v2-token\src\base\ERC1155Enumerable.sol::8 => import {IERC1155Enumerable} from "../interfaces/IERC1155Enumerable.sol";
2023-01-timeswap\packages\v2-token\src\interfaces\IERC1155Enumerable.sol::6 => import "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
2023-01-timeswap\packages\v2-token\src\interfaces\ITimeswapV2Token.sol::4 => import {IERC1155} from "@openzeppelin/contracts/token/ERC1155/IERC1155.sol";
2023-01-timeswap\packages\v2-token\src\structs\FeesPosition.sol::4 => import {FeeCalculation} from "@timeswap-labs/v2-pool/src/libraries/FeeCalculation.sol";
2023-01-timeswap\packages\v2-token\src\structs\FeesPosition.sol::5 => import {Math} from "@timeswap-labs/v2-library/src/Math.sol";
2023-01-timeswap\packages\v2-token\src\structs\Param.sol::4 => import {Error} from "@timeswap-labs/v2-library/src/Error.sol";
2023-01-timeswap\packages\v2-token\src\structs\Position.sol::4 => import {TimeswapV2OptionPosition} from "@timeswap-labs/v2-option/src/enums/Position.sol";
```

### Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: [G008]

#### Findings:
```
2023-01-timeswap\packages\v2-library\src\FullMath.sol::187 => // Make sure the result is less than 2**256.
2023-01-timeswap\packages\v2-library\src\FullMath.sol::223 => // to flip `twos` such that it is 2**256 / twos.
2023-01-timeswap\packages\v2-library\src\FullMath.sol::230 => // Invert divisor mod 2**256
2023-01-timeswap\packages\v2-library\src\FullMath.sol::232 => // modulo 2**256 such that divisor * inv = 1 mod 2**256.
2023-01-timeswap\packages\v2-library\src\FullMath.sol::234 => // correct for four bits. That is, divisor * inv = 1 mod 2**4
2023-01-timeswap\packages\v2-library\src\FullMath.sol::241 => inv *= 2 - divisor * inv; // inverse mod 2**8
2023-01-timeswap\packages\v2-library\src\FullMath.sol::246 => inv *= 2 - divisor * inv; // inverse mod 2**256
2023-01-timeswap\packages\v2-library\src\FullMath.sol::250 => // correct result modulo 2**256. Since the precoditions guarantee
2023-01-timeswap\packages\v2-library\src\FullMath.sol::251 => // that the outcome is less than 2**256, this is the final result.
```