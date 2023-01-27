## [01] USER CAN POSSIBLY TRANSFER NO `token0` OR `token1` TO `TimeswapV2Option` CONTRACT IF CORRESPONDING `token0` OR `token1` IS A REBASING TOKEN
When calling the following `TimeswapV2Option.mint` function, `msg.sender` uses the `ITimeswapV2OptionMintCallback.timeswapV2OptionMintCallback` function to transfer the relevant `token0` and/or `token1` to the `TimeswapV2Option` contract. Similarly, when calling the `TimeswapV2Option.swap` function below, `msg.sender` uses the `ITimeswapV2OptionSwapCallback.timeswapV2OptionSwapCallback` function to transfer the relevant `token0` or `token1` to the `TimeswapV2Option` contract. When `token0` or `token1` is a rebasing token, it is possible that the user uses these callback functions to trigger such token's rebasing event that increases its balance owned by the `TimeswapV2Option` contract. Then, when the `TimeswapV2Option.mint` and `TimeswapV2Option.swap` functions call `Error.checkEnough`, the rebasing token's balance owned by the `TimeswapV2Option` contract can possibly exceed the corresponding balance target. As a result, the user is able to mint or swap option positions without sending any of such rebasing token to the `TimeswapV2Option` contract.

As a mitigation, this protocol can behave like other protocols that do not support rebasing tokens and use a blocklist to block such tokens from being used as `token0` or `token1` for any options.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L109-L154
```solidity
    function mint(
        TimeswapV2OptionMintParam calldata param
    ) external override noDelegateCall returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data) {
        ...

        Option storage option = options[param.strike][param.maturity];

        // does main mint logic calculation
        (token0AndLong0Amount, token1AndLong1Amount, shortAmount) = option.mint(param.strike, param.long0To, param.long1To, param.shortTo, param.transaction, param.amount0, param.amount1);

        // update token0 and token1 balance target for any previous concurrent option transactions.
        processing.updateProcess(token0AndLong0Amount, token1AndLong1Amount, true, true);

        // add a new process
        // stores the token0 and token1 balance target required from the msg.sender to achieve.
        Process storage currentProcess = (processing.push() = Process(
            param.strike,
            param.maturity,
            IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
            IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount
        ));

        // ask the msg.sender to transfer token0 and/or token1 to this contract.
        data = ITimeswapV2OptionMintCallback(msg.sender).timeswapV2OptionMintCallback(
            TimeswapV2OptionMintCallbackParam({
                strike: param.strike,
                maturity: param.maturity,
                token0AndLong0Amount: token0AndLong0Amount,
                token1AndLong1Amount: token1AndLong1Amount,
                shortAmount: shortAmount,
                data: param.data
            })
        );

        // check if the token0 balance target is achieved.
        if (token0AndLong0Amount != 0) Error.checkEnough(IERC20(token0).balanceOf(address(this)), currentProcess.balance0Target);

        // check if the token1 balance target is achieved.
        if (token1AndLong1Amount != 0) Error.checkEnough(IERC20(token1).balanceOf(address(this)), currentProcess.balance1Target);

        ...
    }
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L198-L244
```solidity
    function swap(TimeswapV2OptionSwapParam calldata param) external override noDelegateCall returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, bytes memory data) {
        ...

        Option storage option = options[param.strike][param.maturity];

        // does main swap logic calculation
        (token0AndLong0Amount, token1AndLong1Amount) = option.swap(param.strike, param.longTo, param.isLong0ToLong1, param.transaction, param.amount);

        // update token0 and token1 balance target for any previous concurrent option transactions.
        processing.updateProcess(token0AndLong0Amount, token1AndLong1Amount, !param.isLong0ToLong1, param.isLong0ToLong1);

        // add a new process
        // stores the token0 and token1 balance target required from the msg.sender to achieve.
        Process storage currentProcess = (processing.push() = Process(
            param.strike,
            param.maturity,
            param.isLong0ToLong1 ? IERC20(token0).balanceOf(address(this)) - token0AndLong0Amount : IERC20(token0).balanceOf(address(this)) + token0AndLong0Amount,
            param.isLong0ToLong1 ? IERC20(token1).balanceOf(address(this)) + token1AndLong1Amount : IERC20(token1).balanceOf(address(this)) - token1AndLong1Amount
        ));

        // transfer token to recipient.
        IERC20(param.isLong0ToLong1 ? token0 : token1).safeTransfer(param.tokenTo, param.isLong0ToLong1 ? token0AndLong0Amount : token1AndLong1Amount);

        // ask the msg.sender to transfer token0 or token1 to this contract.
        data = ITimeswapV2OptionSwapCallback(msg.sender).timeswapV2OptionSwapCallback(
            TimeswapV2OptionSwapCallbackParam({
                strike: param.strike,
                maturity: param.maturity,
                isLong0ToLong1: param.isLong0ToLong1,
                token0AndLong0Amount: token0AndLong0Amount,
                token1AndLong1Amount: token1AndLong1Amount,
                data: param.data
            })
        );

        // check if the token0 or token1 balance target is achieved.
        Error.checkEnough(IERC20(param.isLong0ToLong1 ? token1 : token0).balanceOf(address(this)), param.isLong0ToLong1 ? currentProcess.balance1Target : currentProcess.balance0Target);

        ...
    }
```

## [02] `Error.checkEnough` FUNCTION DOES NOT PREVENT USER FROM SENDING TOO MUCH TOKEN TO RELEVANT CONTRACT
When calling function like `TimeswapV2Option.mint` or `TimeswapV2Option.swap`, the user will use the callback function like `ITimeswapV2OptionMintCallback.timeswapV2OptionMintCallback` or `ITimeswapV2OptionSwapCallback.timeswapV2OptionSwapCallback` to send some of the corresponding token to the relevant contract, such as `TimeswapV2Option`. Calling function like `TimeswapV2Option.mint` or `TimeswapV2Option.swap` will revert if its call to the following `Error.checkEnough` function reverts when the token amount transferred through the callback function is not enough. However, if user sends too much token through the callback function, the `Error.checkEnough` function does not revert; when this happens, the extra token amount after the corresponding token balance target is met will be locked in the relevant receiving contract like `TimeswapV2Option` so the user loses such extra amount.

As a mitigation, the `balance < balanceTarget` condition in the `Error.checkEnough` function can be updated to `balance != balanceTarget`. Alternatively, some logics can be added for returning the sent extra token amount back to the user.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol#L151-L153
```solidity
    function checkEnough(uint256 balance, uint256 balanceTarget) internal pure {
        if (balance < balanceTarget) revert NotEnoughReceived(balance, balanceTarget);
    }
```

## [03] POOL CONSIDERS OPTION NOT MATURED WHEN ITS MATURITY AND THE BLOCK TIMESTAMP ARE EQUAL
As shown by the following comparisons between the option's maturity and the block timestamp in the following `TimeswapV2Pool.initialize` function and the pool's various `ParamLibrary.check` functions, the pool considers the corresponding option not matured when its maturity and the block timestamp are equal. However, this is inconsistent with the option's definition, which considers the option matured when its maturity and the block timestamp are equal as the option's various `ParamLibrary.check` functions below show. The `TimeswapV2Pool` contract's `mint`, `burn`, `deleverage`, `leverage`, and `rebalance` functions all have the `can be only called before the maturity` comment indicating that these functions are supposed to be callable only when the corresponding option is not matured. Users who checked these comments can believe that these functions are callable when the option's maturity and the block timestamp are equal; yet, calling these functions at such timing will revert due to the duration to the option's maturity is already 0 and the inconsistency between the pool and option's definitions of whether the option is matured. As a result, in this case, the user's calls of these `TimeswapV2Pool` contract's functions will revert unexpectedly with the used gas being wasted, and the user experience becomes degraded.

As a mitigation, the pool and option's definitions of whether the option is matured need to be updated to be consistent.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L175-L181
```solidity
    function initialize(uint256 strike, uint256 maturity, uint160 rate) external override noDelegateCall {
        if (maturity < blockTimestamp(0)) Error.alreadyMatured(maturity, blockTimestamp(0));
        ...
    }
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L142-L194
```solidity
    ...
    function check(TimeswapV2PoolMintParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        ...
    }

    ...
    function check(TimeswapV2PoolBurnParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        ...
    }

    ...
    function check(TimeswapV2PoolDeleverageParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        ...
    }

    ...
    function check(TimeswapV2PoolLeverageParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        ...
    }

    ...
    function check(TimeswapV2PoolRebalanceParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity < blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        ...
    }
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Param.sol#L103-L151
```solidity
    function check(TimeswapV2OptionMintParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity <= blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        ...
    }

    ...
    function check(TimeswapV2OptionBurnParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity <= blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        ...
    }

    ...
    function check(TimeswapV2OptionSwapParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity <= blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        ...
    }

    ...
    function check(TimeswapV2OptionCollectParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.maturity > blockTimestamp) Error.stillActive(param.maturity, blockTimestamp);
        ...
    }
```

## [04] POOL'S `ParamLibrary.check` FUNCTION FOR `TimeswapV2PoolCollectParam` CHECKS `param.strike == 0` LESS RESTRICTIVELY THAN POOL'S OTHER `ParamLibrary.check` FUNCTIONS
As shown below, calling the pool's `ParamLibrary.check` function for `TimeswapV2PoolCollectParam` will not revert when `param.strike` is 0 while one or more of `param.long0Requested`, `param.long1Requested`, or `param.shortRequested` is not 0. However, calling the pool's other `ParamLibrary.check` functions for other `param` will revert whenever `param.strike == 0` is true. To handle the `param.strike == 0` check consistently, please consider update the `&& param.strike == 0` condition to `|| param.strike == 0` in the pool's `ParamLibrary.check` function for `TimeswapV2PoolCollectParam`.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Param.sol#L133-L194
```solidity
    function check(TimeswapV2PoolCollectParam memory param) internal pure {
        ...
        if (param.long0Requested == 0 && param.long1Requested == 0 && param.shortRequested == 0 && param.strike == 0) Error.zeroInput();
    }

    ...
    function check(TimeswapV2PoolMintParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.delta == 0 || param.strike == 0) Error.zeroInput();
    }

    ...
    function check(TimeswapV2PoolBurnParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.delta == 0 || param.strike == 0) Error.zeroInput();
    }

    ...
    function check(TimeswapV2PoolDeleverageParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.delta == 0 || param.strike == 0) Error.zeroInput();
    }

    ...
    function check(TimeswapV2PoolLeverageParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.delta == 0 || param.strike == 0) Error.zeroInput();
    }

    ...
    function check(TimeswapV2PoolRebalanceParam memory param, uint96 blockTimestamp) internal pure {
        ...
        if (param.delta == 0 || param.strike == 0) Error.zeroInput();
    }
```

## [05] IMPROPER VERIFICATION OF CRYPTOGRAPHIC SIGNATURE VULNERABILITY OF `@openzeppelin/contracts` VERSIONS THAT ARE <4.7.3
As shown in the following `package.json` code, versions that are lower than 4.7.3 of `@openzeppelin/contracts` can be in use for this protocol. As mentioned in https://security.snyk.io/vuln/SNYK-JS-OPENZEPPELINCONTRACTS-2980279, versions that are lower than 4.7.3 of `@openzeppelin/contracts` can suffer from an improper verification of cryptographic signature vulnerability involving `ECDSA.recover` and EIP-2098 compact signatures. To reduce the attack surface and avoid the potential risk, please consider use version 4.7.3 or higher of `@openzeppelin/contracts`.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/package.json#L40
```
"@openzeppelin/contracts": "^4.7.2",
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/package.json#L40
```
"@openzeppelin/contracts": "^4.7.2"
```

## [06] REDUNDANT NAMED RETURNS
When a function has unused named returns and used return statement, these named returns become redundant. To improve readability and maintainability, these variables for the named returns can be removed while keeping the return statements for the functions associated with the following lines.

```solidity
v2-pool\src\TimeswapV2Pool.sol
  240: function mint(TimeswapV2PoolMintParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
  248: ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
  305: function burn(TimeswapV2PoolBurnParam calldata param) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
  313: ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
```

## [07] WORD TYPING TYPOS
`ot` can be changed to `to` in the following comment.
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol#L22
```solidity
    /// @param amount The amount ot be converted. Token0 amount when zeroToOne. Token1 amount when oneToZero.
```

`overidden` can be changed to `overridden` in the following comment.
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L69
```solidity
    // Can be overidden for testing purposes.
```

`positionss` can be changed to `positions` in the following comment.
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L10
```solidity
    /// @dev The liquidity positionss will already be minted to the receipient.
```

## [08] CONFUSING NATSPEC `@param` USAGE
Because `data` is the returned variable of the following functions, `@return` can be used instead of `@param` in the corresponding NatSpec comment to avoid confusion.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L13-L14
```solidity
    /// @param data The bytes of data to be sent to msg.sender.
    function timeswapV2PoolMintChoiceCallback(TimeswapV2PoolMintChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data);
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/callbacks/ITimeswapV2PoolMintCallback.sol#L17-L18
```solidity
    /// @param data The bytes of data to be sent to msg.sender.
    function timeswapV2PoolMintCallback(TimeswapV2PoolMintCallbackParam calldata param) external returns (bytes memory data);
```

## [09] INCOMPLETE NATSPEC COMMENTS
NatSpec comments provide rich code documentation. The following functions are some examples that miss the `@param` and/or `@return` comments. Please consider completing the NatSpec comments for functions like these.
```solidity
v2-library\src\Error.sol
  125: function inactiveOptionChoice(uint256 strike, uint256 maturity) internal pure { 

v2-library\src\StrikeConversion.sol
  16: function convert(uint256 amount, uint256 strike, bool zeroToOne, bool roundUp) internal pure returns (uint256) {    
  26: function turn(uint256 amount, uint256 strike, bool toOne, bool roundUp) internal pure returns (uint256) {    
  35: function combine(uint256 amount0, uint256 amount1, uint256 strike, bool roundUp) internal pure returns (uint256) {  

v2-option\src\interfaces\ITimeswapV2Option.sol
  169: function burn(TimeswapV2OptionBurnParam calldata param) external returns (uint256 token0AndLong0Amount, uint256 token1AndLong1Amount, uint256 shortAmount, bytes memory data);  
  190: function collect(TimeswapV2OptionCollectParam calldata param) external returns (uint256 token0Amount, uint256 token1Amount, uint256 shortAmount, bytes memory data);  

v2-option\src\interfaces\ITimeswapV2OptionFactory.sol
  28: function getByIndex(uint256 id) external view returns (address optionPair); 

v2-pool\src\interfaces\callbacks\ITimeswapV2PoolBurnCallback.sol
  13: function timeswapV2PoolBurnChoiceCallback(TimeswapV2PoolBurnChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data); 

v2-pool\src\interfaces\callbacks\ITimeswapV2PoolMintCallback.sol
  14: function timeswapV2PoolMintChoiceCallback(TimeswapV2PoolMintChoiceCallbackParam calldata param) external returns (uint256 long0Amount, uint256 long1Amount, bytes memory data); 
  18: function timeswapV2PoolMintCallback(TimeswapV2PoolMintCallbackParam calldata param) external returns (bytes memory data); 
```

## [10] MISSING NATSPEC COMMENTS
NatSpec comments provide rich code documentation. The following functions are some examples that miss NatSpec comments. Please consider adding NatSpec comments for functions like these.
```solidity
v2-option\src\TimeswapV2Option.sol
  56: function addOptionEnumerationIfNecessary(uint256 strike, uint256 maturity) private {    
  70: function blockTimestamp() internal view virtual returns (uint96) {  

v2-pool\src\TimeswapV2Pool.sol
  57: function addPoolEnumerationIfNecessary(uint256 strike, uint256 maturity) private { 
  66: function raiseGuard(uint256 strike, uint256 maturity) private { 
  71: function lowerGuard(uint256 strike, uint256 maturity) private { 
  82: function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {    
  252: function mint(  

v2-pool\src\TimeswapV2PoolDeployer.sol
  25: function deploy(address poolFactory, address optionPair, uint256 transactionFee, uint256 protocolFee) internal returns (address poolPair) { 

v2-pool\src\structs\LiquidityPosition.sol
  85: function collectTransactionFees(    
```