# Audit Report

## Summary
[G01] NOT USING THE NAMED RETURN VARIABLES WHEN A FUNCTION RETURNS, WASTES DEPLOYMENT GAS
[G02] HELP THE OPTIMIZER BY SAVING A STORAGE VARIABLE’S REFERENCE INSTEAD OF REPEATEDLY FETCHING IT
[G03] USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD
[G04] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE
[G05] USE A MORE RECENT VERSION OF SOLIDITY
[G06] PACK THE STRUCT VARIABLES
[G07] ABI.ENCODE() IS LESS EFFICIENT THAN ABI.ENCODEPACKED()

## Issues found

### [G01] NOT USING THE NAMED RETURN VARIABLES WHEN A FUNCTION RETURNS, WASTES DEPLOYMENT GAS

#### Findings:
```
packages\v2-pool\src\TimeswapV2Pool.sol::118 => function feeGrowth(uint256 strike, uint256 maturity) external view override returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {
packages\v2-pool\src\TimeswapV2Pool.sol::123 => function feesEarnedOf(uint256 strike, uint256 maturity, address owner) external view override returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
packages\v2-pool\src\TimeswapV2Pool.sol::128 => function protocolFeesEarned(uint256 strike, uint256 maturity) external view override returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {
packages\v2-pool\src\TimeswapV2Pool.sol::240 => function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::248 => ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::305 => function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::313 =>  ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::365 => function deleverage(TimeswapV2PoolDeleverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::373 => ) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::421 => function leverage(TimeswapV2PoolLeverageParam calldata param) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\TimeswapV2Pool.sol::426 => function leverage(TimeswapV2PoolLeverageParam calldata param, uint96 durationForward) external override returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
packages\v2-pool\src\structs\Pool.sol::75 => function feesEarnedOf(Pool storage pool, address owner) external view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
```

#### Tools used
Manual

### [G02] HELP THE OPTIMIZER BY SAVING A STORAGE VARIABLE’S REFERENCE INSTEAD OF REPEATEDLY FETCHING IT
To help the optimizer, declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array.
The effect can be quite significant.
As an example, instead of repeatedly calling someMap[someIndex], save its reference like this: SomeStruct storage someStruct = someMap[someIndex] and use it.

#### Findings:
```
packages\v2-token\src\TimeswapV2LiquidityToken.sol::199 => (long0Fees, long1Fees, shortFees) = _feesPositions[id][msg.sender].getFees(param.long0FeesDesired, param.long1FeesDesired, param.shortFeesDesired);
packages\v2-token\src\TimeswapV2LiquidityToken.sol::216 => _feesPositions[id][msg.sender].burn(long0Fees, long1Fees, shortFees);
```

#### Tools used
Manual

### [G03] USAGE OF UINTS/INTS SMALLER THAN 32 BYTES (256 BITS) INCURS OVERHEAD

#### Findings:
```
packages\v2-pool\src\libraries\ReentrancyGuard.sol::12 => uint96 internal constant NOT_INTERACTED = 0;
packages\v2-pool\src\libraries\ReentrancyGuard.sol::15 => uint96 internal constant NOT_ENTERED = 1;
packages\v2-pool\src\libraries\ReentrancyGuard.sol::18 => uint96 internal constant ENTERED = 2;
```

#### Tools used
Manual

### [G04] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

#### Findings:
```
packages\v2-pool\src\base\OwnableTwoSteps.sol::23 => function setPendingOwner(address chosenPendingOwner) external override {
packages\v2-pool\src\base\OwnableTwoSteps.sol::35 => function acceptOwner() external override {
```

#### Tools used
Manual

### [G05] USE A MORE RECENT VERSION OF SOLIDITY
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.

#### Tools used
Manual

### [G06] Pack the struct variables

Better approach:
```
struct TimeswapV2OptionCollectParam {
    uint256 strike;
    uint256 maturity;
    uint256 amount;
    address token0To;
    address token1To;
    TimeswapV2OptionCollect transaction;
    bytes data;
}
```

#### Findings:
```
packages\v2-option\src\structs\Param.sol::20 => struct TimeswapV2OptionMintParam {
packages\v2-option\src\structs\Param.sol::44 => struct TimeswapV2OptionBurnParam {
packages\v2-option\src\structs\Param.sol::67 => struct TimeswapV2OptionSwapParam {
packages\v2-option\src\structs\Param.sol::89 => struct TimeswapV2OptionCollectParam {
packages\v2-pool\src\structs\CallbackParam.sol::149 => struct TimeswapV2PoolRebalanceCallbackParam {
packages\v2-pool\src\structs\Param.sol::17 => struct TimeswapV2PoolCollectParam {
packages\v2-pool\src\structs\Param.sol::37 => struct TimeswapV2PoolMintParam {
packages\v2-pool\src\structs\Param.sol::57 => struct TimeswapV2PoolBurnParam {
packages\v2-pool\src\structs\Param.sol::78 => struct TimeswapV2PoolDeleverageParam {
packages\v2-pool\src\structs\Param.sol::98 => struct TimeswapV2PoolLeverageParam { 
packages\v2-pool\src\structs\Param.sol::120 => struct TimeswapV2PoolRebalanceParam {
packages\v2-pool\src\structs\Pool.sol::41 => struct Pool {
packages\v2-token\src\structs\Param.sol::18 => struct TimeswapV2TokenMintParam {
packages\v2-token\src\structs\Param.sol::44 => struct TimeswapV2TokenBurnParam {
packages\v2-token\src\structs\Param.sol::66 => struct TimeswapV2LiquidityTokenMintParam {
packages\v2-token\src\structs\Param.sol::84 => struct TimeswapV2LiquidityTokenBurnParam {
packages\v2-token\src\structs\Param.sol::104 => struct TimeswapV2LiquidityTokenCollectParam {
```

#### Tools used
Manual

### [G07] ABI.ENCODE() IS LESS EFFICIENT THAN ABI.ENCODEPACKED()

##### Findings:
```
packages\v2-option\src\TimeswapV2OptionDeployer.sol::35 => optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encode(token0, token1))}());
packages\v2-pool\src\TimeswapV2PoolDeployer.sol::28 => poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());
packages\v2-token\src\TimeswapV2Token.sol::46 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
packages\v2-token\src\TimeswapV2Token.sol::53 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
packages\v2-token\src\TimeswapV2Token.sol::61 => bytes32 key = keccak256(abi.encode(token0, token1, strike, maturity));
packages\v2-token\src\structs\Position.sol::35 => return keccak256(abi.encode(timeswapV2TokenPosition));
packages\v2-token\src\structs\Position.sol::40 => return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
```

#### Tools used
Manual