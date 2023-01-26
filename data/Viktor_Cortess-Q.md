### NC – Some code left from the development

**packages\v2-token\src\TimeswapV2Token.sol**

    5: import "forge-std/console.sol";

    109: console.log("reaches right before mint in timeswapv2Tokne::mint");

    170: console.log()

### NC – non-descriptive function declarations.

Most of the functions in the project have 1-word names like get, create, check, etc. Without Natspec it makes understanding of the code harder.

For example:

**\packages\v2-option\test\src\TimeswapV2OptionFactory.sol**

    33: function get(address token0, address token1) external view override returns (address optionPair) {
    47: function create(address token0, address token1) external override returns (address optionPair) {

### [L-01] UNSPECIFIC COMPILER VERSION PRAGMA

Avoid floating pragmas for non-library contracts.
While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.
A known vulnerable compiler version may accidentally be selected or security tools might fall back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.
It is recommended to pin to a concrete compiler version.

    packages\v2-token\src\interfaces\IERC1155Enumerable.sol => pragma solidity ^0.8.0;
    packages\v2-token\src\base\ERC1155Enumerable.sol => pragma solidity ^0.8.0;
    packages\v2-token\src\TimeswapV2LiquidityToken.sol => pragma solidity ^0.8.8;
    packages\v2-token\src\TimeswapV2Token.sol => pragma solidity ^0.8.8;
    packages\v2-token\src\structs\CallbackParam.sol  => pragma solidity ^0.8.8;
    packages\v2-token\src\structs\FeesPosition.sol => pragma solidity ^0.8.8;
    packages\v2-token\src\structs\Param.sol => pragma solidity ^0.8.8;
    packages\v2-token\src\structs\Position.sol => pragma solidity ^0.8.8;

### [L-02] ADD ZERO ADDRESS VALIDATION IN CONSTRUCTOR 

Parameters used in constructor are used to initialize the state variable, error in these can lead to the redeployment of the contract.

**packages\v2-token\src\TimeswapV2LiquidityToken.sol**

    37: optionFactory = chosenOptionFactory;
    38: poolFactory = chosenPoolFactory;

**packages\v2-token\src\TimeswapV2Token.sol**

    42 : optionFactory = chosenOptionFactory;

**\packages\v2-pool\src\base\OwnableTwoSteps.sol**

    18: constructor(address chosenOwner) {
        owner = chosenOwner;
    }

**\packages\v2-token\src\TimeswapV2Token.sol**

    41: constructor(address chosenOptionFactory) ERC1155("Timeswap V2 address") {

**\packages\v2-token\src\TimeswapV2LiquidityToken.sol**

    36: constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {

**\packages\v2-pool\src\TimeswapV2Pool.sol**

    77: constructor() NoDelegateCall() { 
        (poolFactory, optionPair, transactionFee, protocolFee) = ITimeswapV2PoolDeployer(msg.sender).parameter();
    }



### [L-03] Several functions with the same name in one contract make it hard to understand especially when one mint()/burn() function calls another mint()/burn() function.

**\packages\v2-pool\src\TimeswapV2Pool.sol**

    240: function **mint**(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
    245: function mint(
        TimeswapV2PoolMintParam calldata param,
        uint96 durationForward
    ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
        return mint(param, true, durationForward);
    }
    252: function mint(
        TimeswapV2PoolMintParam calldata param,
        bool isQuote,
        uint96 durationForward
    )
    305: function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
        return burn(param, false, 0);
    }
    310: function burn(
        TimeswapV2PoolBurnParam calldata param,
        uint96 durationForward
    ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
        return burn(param, true, durationForward);
    }
    317: function burn(
        TimeswapV2PoolBurnParam calldata param,
        bool isQuote,
        uint96 durationForward
    )

    365: function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
        return deleverage(param, false, 0);
    }

    370: function deleverage(
        TimeswapV2PoolDeleverageParam calldata param,
        uint96 durationForward
    ) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
        return deleverage(param, true, durationForward);
    }
    377: function deleverage(
        TimeswapV2PoolDeleverageParam calldata param,
        bool isQuote,
        uint96 durationForward
    )

    421: function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
        return leverage(param, false, 0);
    }

    /// @inheritdoc ITimeswapV2Pool
    function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
        return leverage(param, true, durationForward);
    }

    function leverage(
        TimeswapV2PoolLeverageParam calldata param,
        bool isQuote,
        uint96 durationForward
    )

### [Low-04] AVOID NESTED IF BLOCKS

For better readability and analysis it is better to avoid nested if blocks. Here is an example:

**\packages\v2-token\src\TimeswapV2LiquidityToken.sol**

    237: - if (from != to) { 
         + require(from != to,””);


