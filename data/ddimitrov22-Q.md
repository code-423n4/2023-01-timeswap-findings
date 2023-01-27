## Overview

The code is split across many contracts, making it easier to read and harder to follow the logic simultaneously. However, the comments are clear and understandable and the documentation is sufficient to grasp the application logic. This report is non-critical and will present findings that do not directly impact the smart contracts' functionality.

## [L-01] USING VULNERABLE DEPENDENCY OF OPENZEPPELIN

Inside `ERC1155Enumarable.sol` it can be seen that the project is using v4.4.1 of OZ which has a not last updated version.

`// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts v4.4.1 (token/ERC721/extensions/ERC721Enumerable.sol)`

**Recommendation**
Use patched versions
Latest non-vulnerable version 4.8.0

## [N-01] PREFER BATTLE-TESTED CODE OVER REIMPLEMENTING COMMON PATTERNS

Replace `ReentrancyGuard.sol` library with the one from OpenZeppelin, since it is well-tested and optimized.

## [N-02] DIFFERENT FUNCTIONS WITH THE SAME NAMES

There are several occasions where different functions with the same names are used. Despite the fact that the functions take different parameters and have different visibility, it makes the code harder to read and it might be misleading. For example: 

        function transferFees(uint256 strike, uint256 maturity, address to, uint256 
        long0Fees, uint256 long1Fees, uint256 shortFees) external override {
        if (to == address(0)) Error.zeroAddress();
        if (long0Fees == 0 && long1Fees == 0 && shortFees == 0) Error.zeroInput();

        pools[strike][maturity].transferFees(maturity, to, long0Fees, long1Fees, shortFees, blockTimestamp(0));

        emit TransferFees(strike, maturity, msg.sender, to, long0Fees, long1Fees, shortFees);
    }

Inside the `transferFees()` another function called `transferFees()` is called. This occurs inside `TimeswapV2Pool.sol`.

Another example is inside `TimeswapV2Option.sol` :

        function transferPosition(uint256 strike, uint256 maturity, address to, TimeswapV2OptionPosition position, uint256 amount) external override {
        if (!hasInteracted[strike][maturity]) Error.inactiveOptionChoice(strike, maturity);
        if (to == address(0)) Error.zeroAddress();
        if (amount == 0) Error.zeroInput();
        PositionLibrary.check(position);

        options[strike][maturity].transferPosition(to, position, amount);

        emit TransferPosition(strike, maturity, msg.sender, to, position, amount);
    }

Here, again inside `transferPosition() another function is called with the same name.

**Recommendation**
Consider renaming the functions so it is easier to differentiate them.

## [N-03] UNUSED IMPORTS

Inside `TimeswapV2Option.sol` the imported enums and library from `Transaction.sol` are unused. Consider removing it.

## [N-04] UNUSED LIBRARIES

`BytesLib.sol` and `CatchError.sol` stored inside `v2-library/src` are not used anywhere across the whole project. Consider removing them.

## [N-05] LOCK *PRAGMAS* TO SPECIFIC COMPILER VERSION

`TimeswapV2LiquidityToken.sol` and `TimeswapV2Token.sol` use floating pragmas `^0.8.8;`. This is also true for all the contracts inside`v2-token/src/structs`. As a best practice, it is always better to lock the pragma to a specific version as in the rest of the contracts.

#[N-06] USE A MORE RECENT VERSION OF SOLIDITY

Consider upgrading the version to a newer one to use bugfixes and optimizations in the compiler. The version used actors the contracts is `0.8.8;` while the latest version is `0.8.17;`

## [N-06] IRRELEVANT COMMENT

Inside the `mint()` function in `TimeswapV2Token.sol` there is an irrelevant comment `// console.log()`. Check it if it is an open todo or remove it.

## [N-07] EVENT IS NOT EMITTED FOR CRITICAL STATE CHANGES

When the `collect()` function is executed inside `TimeswapV2LiquidityToken.sol` and the fees are sent to the recipient through calling `transferFees()` an event is not emitted. This could be problematic for off-chain monitoring. Even though another event is emitted further down in the function when the `burn()` function is called, this is not. sufficient as the event emitted through simply executing `burn()` will be exactly the same and the transferred fees cannot be monitored.

**Recommendation**
Consider emitting and event for state changes.