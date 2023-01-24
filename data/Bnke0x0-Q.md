

### [L01] `initialize` functions can be front-run

#### Impact
See [this](https://github.com/code-423n4/2021-10-badgerdao-findings/issues/40) finding from a prior badger-dao contest for details
#### Findings:
```
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::175 => function initialize(uint256 strike, uint256 maturity, uint160 rate) external override noDelegateCall {
2023-01-timeswap-main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol::252 => function initialize(uint256 strike, uint256 maturity, uint160 rate) external;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::205 => function initialize(Pool storage pool, uint160 rate) external {
```





### [L02] Unspecific Compiler Version Pragma

#### Impact
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.
#### Findings:
```
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap-main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap-main/packages/v2-token/src/structs/CallbackParam.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/FeesPosition.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/Param.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/Position.sol::2 => pragma solidity ^0.8.8;
```



### Non-Critical Issues


### [N01] Adding a return statement when the function defines a named return variable, is redundant


#### Findings:
```
2023-01-timeswap-main/packages/v2-library/src/BytesLib.sol::70 => return tempBytes;
```



### [N02] `require()`/`revert()` statements should have descriptive reason strings


#### Findings:
```
2023-01-timeswap-main/packages/v2-library/src/CatchError.sol::18 => revert(reason, length)
```

### [N03] Use a more recent version of solidity


#### Findings:
```
2023-01-timeswap-main/packages/v2-library/src/BytesLib.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/CatchError.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/Error.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/FullMath.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/Math.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/Ownership.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/SafeCast.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-library/src/StrikeConversion.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2OptionDeployer.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2OptionFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/enums/Position.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/enums/Transaction.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/ITimeswapV2Option.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/ITimeswapV2OptionDeployer.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/ITimeswapV2OptionFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/NoDelegateCall.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionBurnCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionCollectCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionMintCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/interfaces/callbacks/ITimeswapV2OptionSwapCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/libraries/OptionFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/libraries/OptionPair.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/libraries/Proportion.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/CallbackParam.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/Option.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/Param.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/Process.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-option/src/structs/StrikeAndMaturity.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/NoDelegateCall.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2PoolFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/base/OwnableTwoSteps.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/enums/Transaction.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/ITimeswapV2Pool.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/ITimeswapV2PoolDeployer.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/ITimeswapV2PoolFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolBurnCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolDeleverageCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolLeverageCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolRebalanceCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantSum.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/Duration.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/DurationCalculation.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/DurationWeight.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/Fee.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/FeeCalculation.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/PoolFactory.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/PoolPair.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/structs/CallbackParam.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/structs/LiquidityPosition.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/structs/Param.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap-main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap-main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2LiquidityTokenMintCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/interfaces/callbacks/ITimeswapV2TokenMintCallback.sol::2 => pragma solidity =0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/CallbackParam.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/FeesPosition.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/Param.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/Position.sol::2 => pragma solidity ^0.8.8;
```



### [N04] Use a more recent version of solidity

#### Impact
Use a solidity version of at least `0.8.13` to get the ability to use using for with a list of free functions
#### Findings:
```
2023-01-timeswap-main/packages/v2-library/src/FullMath.sol::7 => using Math for uint256;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::33 => using OptionLibrary for Option;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::34 => using ProcessLibrary for Process[];
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::35 => using Math for uint256;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2Option.sol::36 => using SafeERC20 for IERC20;
2023-01-timeswap-main/packages/v2-option/src/TimeswapV2OptionFactory.sol::18 => using SafeERC20 for IERC20;
2023-01-timeswap-main/packages/v2-option/src/libraries/OptionFactory.sol::11 => using OptionPairLibrary for address;
2023-01-timeswap-main/packages/v2-option/src/structs/Option.sol::30 => using Math for uint256;
2023-01-timeswap-main/packages/v2-option/src/structs/Option.sol::31 => using Proportion for uint256;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::37 => using PoolLibrary for Pool;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::38 => using Ownership for address;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2Pool.sol::39 => using LiquidityPositionLibrary for LiquidityPosition;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2PoolFactory.sol::17 => using OptionPairLibrary for address;
2023-01-timeswap-main/packages/v2-pool/src/TimeswapV2PoolFactory.sol::18 => using Ownership for address;
2023-01-timeswap-main/packages/v2-pool/src/base/OwnableTwoSteps.sol::11 => using Ownership for address;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::13 => using Math for uint256;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantProduct.sol::14 => using SafeCast for uint256;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ConstantSum.sol::12 => using Math for uint256;
2023-01-timeswap-main/packages/v2-pool/src/libraries/DurationCalculation.sol::11 => using Math for uint256;
2023-01-timeswap-main/packages/v2-pool/src/libraries/DurationCalculation.sol::12 => using SafeCast for uint256;
2023-01-timeswap-main/packages/v2-pool/src/libraries/DurationWeight.sol::8 => using Math for uint256;
2023-01-timeswap-main/packages/v2-pool/src/libraries/FeeCalculation.sol::9 => using Math for uint256;
2023-01-timeswap-main/packages/v2-pool/src/libraries/PoolFactory.sol::11 => using OptionFactoryLibrary for address;
2023-01-timeswap-main/packages/v2-pool/src/structs/LiquidityPosition.sol::26 => using Math for uint256;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::57 => using LiquidityPositionLibrary for LiquidityPosition;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::58 => using Math for uint256;
2023-01-timeswap-main/packages/v2-pool/src/structs/Pool.sol::59 => using SafeCast for uint256;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::28 => using ReentrancyGuard for uint96;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::30 => using PositionLibrary for TimeswapV2LiquidityTokenPosition;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::31 => using FeesPositionLibrary for FeesPosition;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::30 => using ReentrancyGuard for uint96;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::32 => using PositionLibrary for TimeswapV2TokenPosition;
```



### [N05] Event is missing `indexed` fields

#### Impact
Each event should use three indexed fields if there are three or more fields

#### Findings:
```

2023-01-timeswap-main/packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol::13 => event TransferFees(address from, address to, TimeswapV2LiquidityTokenPosition position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees);
```


### [N06] States/flags should use Enums rather than separate constants

#### Impact
Issue Information: [NC013](https://github.com/Bnke0x0/c4-common-issues/blob/main/2-Low-Risk.md#n013---States/flags-should-use-Enums-rather-than-separate-constants)

#### Findings:
```
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::12 => uint96 internal constant NOT_INTERACTED = 0;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::15 => uint96 internal constant NOT_ENTERED = 1;
2023-01-timeswap-main/packages/v2-pool/src/libraries/ReentrancyGuard.sol::18 => uint96 internal constant ENTERED = 2;
```


### [N07] Unused file


#### Findings:
```
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::1 => // SPDX-License-Identifier: MIT
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::1 => // SPDX-License-Identifier: MIT
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::1 => // SPDX-License-Identifier: MIT
2023-01-timeswap-main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol::1 => // SPDX-License-Identifier: MIT
2023-01-timeswap-main/packages/v2-token/src/structs/CallbackParam.sol::1 => // SPDX-License-Identifier: MIT
2023-01-timeswap-main/packages/v2-token/src/structs/FeesPosition.sol::1 => // SPDX-License-Identifier: MIT
2023-01-timeswap-main/packages/v2-token/src/structs/Param.sol::1 => // SPDX-License-Identifier: MIT
2023-01-timeswap-main/packages/v2-token/src/structs/Position.sol::1 => // SPDX-License-Identifier: MIT
```



### [N08] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external .

#### Findings:
```
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::66 => function positionOf(address owner, TimeswapV2TokenPosition calldata timeswapV2TokenPosition) public view returns (uint256 amount) {
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::36 => function totalSupply() public view override returns (uint256) {
```




### [N09] NC-library/interface files should use fixed compiler versions, not floating ones


#### Findings:
```
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2LiquidityToken.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/TimeswapV2Token.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/base/ERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap-main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol::4 => pragma solidity ^0.8.0;
2023-01-timeswap-main/packages/v2-token/src/structs/CallbackParam.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/FeesPosition.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/Param.sol::2 => pragma solidity ^0.8.8;
2023-01-timeswap-main/packages/v2-token/src/structs/Position.sol::2 => pragma solidity ^0.8.8;
```






