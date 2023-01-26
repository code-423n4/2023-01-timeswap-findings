### [G-01] Some functions with a lot of checks can be simplified to save some gas 

**1) packages\v2-option\typechain\test\src\structs\Param.sol**

    103 : function check(TimeswapV2OptionMintParam memory param, uint96 blockTimestamp) internal pure {
        if (param.strike == 0) Error.zeroInput();
        if (param.maturity > type(uint96).max) Error.incorrectMaturity(param.maturity);
        if (param.maturity <= blockTimestamp) Error.alreadyMatured(param.maturity, blockTimestamp);
        if (param.shortTo == address(0)) Error.zeroAddress();
        if (param.long0To == address(0)) Error.zeroAddress();
        if (param.long1To == address(0)) Error.zeroAddress();
       -  TransactionLibrary.check(param.transaction);
       +  if (uint256(param.transaction) >= 2) revert InvalidTransaction();
        if (param.amount0 == 0 && param.amount1 == 0) Error.zeroInput();
    }
We can replace this line: `110: TransactionLibrary.check(param.transaction);`  With a simple if statement:

`if (uint256(param.transaction) >= 2) revert InvalidTransaction();`

Results of the test:
Before: (gas: 816)
After: (gas: 787)

Similar situation to the other 3 `check()` functions in ParamLibrary.



**2) packages\v2-option\typechain\test\src\TimeswapV2OptionFactory.sol** 

    46: function create(address token0, address token1) external override returns (address optionPair) {
        if (token0 == address(0)) Error.zeroAddress(); 
        if (token1 == address(0)) Error.zeroAddress();

      - OptionPairLibrary.checkCorrectFormat(token0, token1); 
      + if (token0 >= token1) revert Error.InvalidOptionPair(token0, token1);

        optionPair = optionPairs[token0][token1];

      - OptionPairLibrary.checkDoesNotExist(token0, token1, optionPair);
      + if (optionPair != address(0)) revert Error.OptionPairAlreadyExisted(token0, token1, optionPair);

        optionPair = deploy(address(this), token0, token1);

        optionPairs[token0][token1] = optionPair;

        emit Create(msg.sender, token0, token1, optionPair);
    }

    33: function get(address token0, address token1) external view override returns (address optionPair) {

       - OptionPairLibrary.checkCorrectFormat(token0, token1);
       + if (token0 >= token1) revert Error.InvalidOptionPair(token0, token1);

        optionPair = optionPairs[token0][token1];
    }
Gas before: AVG 1553
Gas after: AVG 1519


### [G-02] USE NAMED RETURNS WHERE APPROPRIATE

**packages\v2-library\src\BytesLib.sol**

`12: function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory)`



### [G-03] In general, using the "variableName = 0" is cheaper in terms of gas consumption than using the "delete" keyword.

When you use the "delete" keyword, the EVM will remove the storage slot associated with the variable, and shift all of the storage slots that come after it. This process consumes more gas than a simple assignment operation such as "variableName = 0", which only requires updating the value of the variable to zero.

**packages\v2-pool\src\base\OwnableTwoSteps.sol**

`39: delete pendingOwner;`

**packages\v2-option\src\TimeswapV2OptionDeployer.sol**

`38: delete parameter;`

**\packages\v2-pool\src\TimeswapV2PoolDeployer.soll**

`30: delete parameter;`



### [G-04] X = X + Y IS MORE EFFICIENT, THAN X += Y (6 INSTANCES)

**packages\v2-pool\src\libraries\ConstantProduct.sol**

`149: if (isAdd) longAmount -= fees;`

`150: else shortAmount -= fees;`

`180: shortAmount -= fees;`

`212: longAmount -= fees;`

`244: amount -= fees;`

`295: denominator2 += longAmount;`



### [G-05] Several if statements can be unified to spend less gas.

In the original code, the contract checks the value of both token0 and token1 separately, executing the Error.zeroAddress() function twice if either of them is equal to address(0). This means that the contract will consume more gas to execute the comparison and the function call twice.

**packages\v2-option\src\TimeswapV2OptionFactory.sol**

`44: if (token0 == address(0)) Error.zeroAddress();`
`45: if (token1 == address(0)) Error.zeroAddress();`

In we unify several if statements, the contract checks the value of both token0 and token1 together using the OR logical operator. If either of them is equal to address(0), the Error.zeroAddress() function is called once. This means that the contract will consume less gas to execute the comparison and the function call once.

`if (token0 == address(0) || token1 == address(0) ) Error.zeroAddress();`


**packages\v2-option\src\structs\Param.sol**

    107: if (param.shortTo == address(0)) Error.zeroAddress();
            if (param.long0To == address(0)) Error.zeroAddress();
            if (param.long1To == address(0)) Error.zeroAddress();

    134: if (param.tokenTo == address(0)) Error.zeroAddress();
            if (param.longTo == address(0)) Error.zeroAddress();

    131: if (param.strike == 0) Error.zeroInput();
    137: if (param.amount == 0) Error.zeroInput();

    147: if (param.token0To == address(0)) Error.zeroAddress(); 
    148: if (param.token1To == address(0)) Error.zeroAddress();

    144: if (param.strike == 0) Error.zeroInput();
    150: if (param.amount == 0) Error.zeroInput();

**\packages\v2-token\src\TimeswapV2LiquidityToken.sol**

    76: if (from == address(0)) Error.zeroAddress();
        if (to == address(0)) Error.zeroAddress();


### [G-06] CHANGING UINT96 TO UINT8 IN library ReentrancyGuard SAVES SOME GAS

In general, it is cheaper to read a uint8 variable than a uint96 variable because uint8 requires less storage than uint96

**\packages\v2-pool\src\libraries\ReentrancyGuard.sol**

    12:  uint96 internal constant NOT_INTERACTED = 0;  //@GAS UINT96 - 32 from TimeswapV2LiquidityToken

    /// @dev The initial and ending state of balanceTarget in the Option struct.
    uint96 internal constant NOT_ENTERED = 1;

    /// @dev The state where the contract is currently being interacted with.
    uint96 internal constant ENTERED = 2;
    function check(uint96 reentrancyGuard) internal pure {
        if (reentrancyGuard == NOT_INTERACTED) revert NotInteracted();
        if (reentrancyGuard == ENTERED) revert NoReentrantCall();
    }
DEPLOYMENT COST BEFORE: 49099, AFTER: 45499
CHECK FUNCTION. BEFORE: 326, AFTER: 290

### [G-07] Consider direct call of errors from Errors.sol

Error.sol file consists of errors and functions with the same names and  these functions inside. For example:

    error ZeroInput();
    function zeroInput() internal pure { 
        revert ZeroInput();
    }
Direct use of errors without functions saves some gas. For example: 

**\packages\v2-pool\src\TimeswapV2Pool.sol**

    152: function transferLiquidity(uint256 strike, uint256 maturity, address to, uint160 liquidityAmount) external override {
        hasLiquidity(strike, maturity);

        if (blockTimestamp(0) > maturity)   Error.alreadyMatured(maturity, blockTimestamp(0));
        if (to == address(0)) Error.zeroAddress();
     -  if (liquidityAmount == 0)   Error.zeroInput(); //Calling a function with error inside
     +  if (liquidityAmount == 0)  revert   Error.ZeroInput(); // Direct call to the same error
        pools[strike][maturity].transferLiquidity(to, liquidityAmount, blockTimestamp(0));

        emit TransferLiquidity(strike, maturity, msg.sender, to, liquidityAmount);
    }

Similar situation with libraries that hold only functions with one if statement and an error inside:



**\packages\v2-pool\src\libraries\PoolPair.sol**

    15: function checkNotZeroAddress(address poolPair) internal pure { 
        if (poolPair == address(0)) revert ZeroSwapAddress();
    }

