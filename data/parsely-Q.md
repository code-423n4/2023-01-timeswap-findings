-  On all contracts the Solidity version should be upgraded to the latest version 0.8.17
-  The transferPosition function in the [TimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol) contract 
```    /// @inheritdoc ITimeswapV2Option
    function transferPosition(uint256 strike, uint256 maturity, address to, TimeswapV2OptionPosition position, uint256 amount) external override {
        if (!hasInteracted[strike][maturity]) Error.inactiveOptionChoice(strike, maturity);
        if (to == address(0)) Error.zeroAddress();
        if (amount == 0) Error.zeroInput();
        PositionLibrary.check(position);

        options[strike][maturity].transferPosition(to, position, amount);

        emit TransferPosition(strike, maturity, msg.sender, to, position, amount);
    } 
``` 
does not transfer any actual funds between the Position holder and the new Postion holder, it changes state variables within the TimeSwap contract, it is up to the the Position holder to recover their input funds via another transaction before transferring the Position to the ``to`` address. This could lead to human error and loss of funds via an error in the ```to``` address or the transfer happening before funds are actually recovered. This is not an error in itself and is by design, but may be worth implementing an "intermediary" function to protect users.