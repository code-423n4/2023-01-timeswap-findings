# 1. Incorrect handling of `uint256` maturity in the Error library

Link: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Error.sol#L50

Summary: 

The `Error` library in the provided code contains several functions that revert execution with an error message when a given maturity value is not within the range of `uint96`. However, the parameter type for the `maturity` value in these functions is declared as `uint256`, which can hold a larger range of values than `uint96`. This means that the revert message will not be triggered when a maturity value is actually outside of the intended range.

Impact: 

This vulnerability could allow for unexpected behavior in contracts that utilize the Error library. Specifically, if a maturity value is passed that is outside of the intended range, no error will be raised, potentially leading to incorrect calculations or other unintended consequences.

Recommendation: 

The parameter type for the `maturity` value in the relevant functions should be updated to `uint96` to correctly handle the intended range of values for maturity. Additionally, it's better to use `assert()` instead of `revert()` as it will give more informative error message.

Example:

```
function alreadyMatured(uint96 maturity, uint96 blockTimestamp) internal pure {
    assert(maturity <= blockTimestamp, "Maturity date has already passed");
}
```

# 2. Unchecked Overflow and Underflow in Math Library 

Link : https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L4

Summary:

The `Math` library contains several functions, such as `unsafeAdd`, `unsafeSub`, and `unsafeMul`, that perform arithmetic operations on `uint256` values without checking for overflow or underflow. Additionally, the `sqrt` and `shr` functions do not check for divide by zero.

Impact: 

These unchecked arithmetic operations can lead to unexpected results and potential security vulnerabilities in contracts that use this library. For example, an attacker could manipulate the input to these functions to cause overflow or underflow, leading to unexpected and potentially exploitable behavior.

Recommendation: 

Use the built-in `SafeMath` library or a similar library that properly checks for overflow and underflow in arithmetic operations. Additionally, check for divide by zero before performing the operation.

Example: 

Instead of using the `unsafeAdd` function, use the `safeAdd` function from the `SafeMath` library, which automatically checks for overflow:

```
import "https://github.com/OpenZeppelin/openzeppelin-solidity/contracts/math/SafeMath.sol";

library Math {
    using SafeMath for uint256;
    function safeAdd(uint256 addend1, uint256 addend2) internal pure returns (uint256 sum) {
        return addend1.add(addend2);
    }
}
```

I want to give another info for this code. The div function does not check for divide by zero, which means it may cause a runtime exception if the divisor is zero. This can be fixed by adding a require statement before the division operation to check if the divisor is not zero.
e.g

```
function div(uint256 dividend, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
    require(divisor != 0, "Divide by zero");
    quotient = dividend / divisor;

    if (roundUp && dividend % divisor != 0) quotient++;
}
```

Also, the `sqrt` function does not check for divide by zero when the input value is zero. So, it should be handled separately.
e.g : 

```
function sqrt(uint256 value, bool roundUp) internal pure returns (uint256 result) {
    if (value == type(uint256).max) return result = type(uint128).max;
    if (value == 0) return 0;
    require(value != 0, "Divide by zero"); 
    unchecked {
        uint256 estimate = (value + 1) >> 1;
        result = value;
        while (estimate < result) {
            result = estimate;
            estimate = (value / estimate + estimate) >> 1;
        }
    }

    if (roundUp && value % result != 0) result++;
}
```

Also, the `shr` function does not check for divide by zero when the input value is zero. So, it should be handled separately.
e.g: 

```
function shr(uint256 dividend, uint8 divisorBit, bool roundUp) internal pure returns (uint256 quotient) {
    require(divisorBit != 0, "Divide by zero");
    quotient = dividend >> divisorBit;

    if (roundUp && dividend % (1 << divisorBit) != 0) quotient++;
}
```

You can say that the `sqrt` and `shr` functions return 0 when the input value is 0, which would avoid a divide by zero error. However, it's still a good practice to include an explicit check for this case and return an error message, so that it's clear to other developers that this is an expected behavior and not a bug.

# 3. Inconsistent Variable Types in TimeswapV2PoolDeleverageChoiceCallbackParam Struct 

Link: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/CallbackParam.sol#L83

Summary:

The variable `longAmount` in the `struct TimeswapV2PoolDeleverageChoiceCallbackParam` has a different `type (uint256)` than the corresponding variables in the other `structs (uint256 long0Amount and uint256 long1Amount)`. This can lead to confusion and potential errors in the code.

Impact: 

Inconsistency in variable types can lead to confusion and potential errors in the code, making it harder to understand and maintain. This could also cause issues when trying to compare or use the variable in question, resulting in unexpected behavior.

Recommendation: 

To prevent confusion and potential errors, update the variable `longAmount` in the `struct TimeswapV2PoolDeleverageChoiceCallbackParam` to match the corresponding variables in the other structs, either by renaming the variable to `long0Amount` and `long1Amount` or by changing its type to `uint256 long0Amount` and `uint256 long1Amount`

Example:

```
struct TimeswapV2PoolDeleverageChoiceCallbackParam {
    uint256 strike;
    uint256 maturity;
    uint256 long0Amount;
    uint256 long1Amount;
    uint256 shortAmount;
    bytes data;
}
```

This will make the code more readable and avoid confusion and errors.

# 4. Old solidity version.

Please upgrade almost all contracts latest solidity version.

# 5. `UnsafeAdd()` function vulnerability in LiquidityPositionLibrary

Links:https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L33
         https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L51

Summary:

The code uses the `unsafeAdd()` function in the `feesEarnedOf()` and `update()` functions within the `LiquidityPositionLibrary`. This function can cause integer overflow and underflow, potentially leading to unexpected results and security vulnerabilities.

Impact:

An attacker could potentially exploit this vulnerability by manipulating the input values to cause integer overflow or underflow, leading to unexpected behavior in the contract such as the transfer of large amounts of assets. This could result in financial loss for users of the contract.

Recommendation:

It is recommended to replace the `unsafeAdd()` function with a safe alternative such as the `safeAdd()` function from the OpenZeppelin library, which prevents integer overflow and underflow.

Example:

Replace the following lines of code:

```
long0Fee = liquidityPosition.long0Fees.unsafeAdd(FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth));
long1Fee = liquidityPosition.long1Fees.unsafeAdd(FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth));
shortFee = liquidityPosition.shortFees.unsafeAdd(FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth));
```

With:

```
import { SafeMath } from '@openzeppelin/contracts';

long0Fee = SafeMath.add(liquidityPosition.long0Fees, FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth));
long1Fee = SafeMath.add(liquidityPosition.long1Fees, FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth));
shortFee = SafeMath.add(liquidityPosition.shortFees, FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth));
```

and

```
liquidityPosition.long0Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth);
liquidityPosition.long1Fees += FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth);
liquidityPosition.shortFees += FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth);
```

with

```
liquidityPosition.long0Fees = SafeMath.add(liquidityPosition.long0Fees, FeeCalculation.getFees(liquidity, liquidityPosition.long0FeeGrowth, long0FeeGrowth));
liquidityPosition.long1Fees = SafeMath.add(liquidityPosition.long1Fees, FeeCalculation.getFees(liquidity, liquidityPosition.long1FeeGrowth, long1FeeGrowth));
liquidityPosition.shortFees = SafeMath.add(liquidityPosition.shortFees, FeeCalculation.getFees(liquidity, liquidityPosition.shortFeeGrowth, shortFeeGrowth));
```

# 6.  Input Validation Vulnerability in FeeCalculation Library

Link :  https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L27

Summary: 

The FeeCalculation library is vulnerable to an input validation issue in its update function. Specifically, the code does not properly validate the value of the `protocolFee` parameter which can be passed with any value, potentially causing unexpected behavior in the calculations.

Impact: 

An attacker could potentially exploit this vulnerability by passing unexpected values for the `protocolFee` parameter, leading to incorrect calculations or unexpected revert messages.

Recommendation: 

To address this vulnerability, the `protocolFee` parameter should be validated to ensure that it falls within a valid range before any calculations are performed.

Example: 

Consider the following example, where an attacker passes a value of 0 for `protocolFee`:

```
function exploit(uint256 protocolFee) {
    uint256 liquidity = 100;
    uint256 feeGrowth = 0;
    uint256 protocolFees = 0;
    uint256 fees = 100;

    (uint256 newFeeGrowth, uint256 newProtocolFees) = FeeCalculation.update(liquidity, feeGrowth, protocolFees, fees, protocolFee);
}
```

In this case, the exploit function would throw a `revert` message because the `getFeesRemoval` function will attempt to divide by zero.
To fix the input validation issue, you can add a check on the `protocolFee` parameter to ensure that it falls within a valid range before any calculations are performed. Here's an example of how you can modify the `update` function to include this check:

```
function update(uint160 liquidity, uint256 feeGrowth, uint256 protocolFees, uint256 fees, uint256 protocolFee) internal pure returns (uint256 newFeeGrowth, uint256 newProtocolFees) {

    require(protocolFee > 0 && protocolFee <= (1 << 16), "Invalid protocol fee rate"); // check if protocolFee is within a valid range (0 < protocolFee <= 2^16)

    uint256 protocolFeesToAdd = getFeesRemoval(fees, protocolFee);

    newFeeGrowth = feeGrowth.unsafeAdd(getFeeGrowth(fees.unsafeSub(protocolFeesToAdd), liquidity));

    newProtocolFees = protocolFees + protocolFeesToAdd;
}
```

# 7. Zero address vulnerability in PoolFactoryLibrary contract

Link: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L43

Summary:

The PoolFactoryLibrary contract contains a vulnerability where the `getWithCheck` function can return a zero address for the `poolPair` variable. This can occur when the `optionPair` variable, retrieved from the `optionFactory` contract, is also a zero address. In this case, the `getWithCheck` function will call the `Error.zeroAddress()` function, which will revert the transaction, but will not prevent the zero address from being returned.

Impact:

This vulnerability can cause the `poolPair` variable to be a zero address, which can lead to further issues in any contracts that rely on the `poolPair` variable being a non-zero address. This can include issues with token transfers and smart contract interactions.

Recommendation:

A recommended fix would be to add a check for a zero address after retrieving the `optionPair` variable and before retrieving the `poolPair` variable. If the `optionPair` variable is a zero address, the `getWithCheck` function should revert the transaction and not return a zero address for the `poolPair` variable.

Example:

```
function getWithCheck(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {
        optionPair = optionFactory.getWithCheck(token0, token1);
        if (optionPair == address(0)) revert();
        poolPair = ITimeswapV2PoolFactory(poolFactory).get(optionPair);
        if (poolPair == address(0)) Error.zeroAddress();
}
```

# 8.  Incorrect handling of zero address in PoolFactoryLibrary

Links:https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L43
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/libraries/OptionFactory.sol#L37

Summary: 

The function `getWithCheck` in the `PoolFactoryLibrary` contract incorrectly handles the case where the returned pool pair address is zero. Instead of using the `Error.zeroAddress()` function to revert, it should use the `revert` keyword to prevent further execution.

Impact: 

If the returned pool pair address is zero, the contract will continue execution and may cause unexpected behavior or errors. This could result in loss of funds or other unintended consequences.

Recommendation: 

Replace the line `if (poolPair == address(0)) Error.zeroAddress();` with `if (poolPair == address(0)) revert();` in the `getWithCheck` function in the PoolFactoryLibrary contract.

Example:

```
function getWithCheck(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {
        optionPair = optionFactory.getWithCheck(token0, token1);

        poolPair = ITimeswapV2PoolFactory(poolFactory).get(optionPair);

        if (poolPair == address(0)) revert();
    }
```

# 9. Unsafe Subtraction in `calculateGivenLongIn()` function

Link: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantSum.sol#L19

Summary: 

The `calculateGivenLongIn()` function in the `ConstantSum` library uses the `unsafeSub()` function to subtract the fees from the `longAmountOut` variable. This can cause an underflow, resulting in a loss of funds.

Impact: 

This vulnerability could result in a loss of funds for the users of the library.

Recommendation: 

Replace the `unsafeSub()` function with a safe subtraction method, such as checking for an underflow before making the subtraction. For example:

```
if(longAmountOut >= longFees){
    longAmountOut = longAmountOut.sub(longFees);
} else {
    // handle error, return 0
    longAmountOut = 0;
}
```

Example:

```
longAmountOut = 100;
longFees = 200;
longAmountOut = longAmountOut.unsafeSub(longFees); // longAmountOut = 100 - 200 = -100, this will result in an underflow
```

or maybe use 

```
require(transactionFee <= longAmountOut, "Transaction fee must be less than longAmountOut");
```

# 10.  Incorrect Fee Initialization Check in FeeLibrary

Link: 
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/Fee.sol#L10

Summary: 

The `check()` function in the `FeeLibrary` library checks if the fee passed as a parameter is greater than the maximum value of a `uint16`. However, this check is incorrect, as the maximum value of a `uint256` is much greater than that of a `uint16`.

Impact: 

This vulnerability would allow a user to pass an incorrectly high fee value, potentially resulting in unintended behavior or loss of funds.

Recommendation: 

Replace the check for `if (fee > type(uint16).max)` with `if (fee > type(uint256).max)`.
e.g

```
uint256 fee = 99999999999999999999;
check(fee); // This function would incorrectly pass, as the fee is less than type(uint16).max
```

# 11. Unchecked index increment in ERC1155Enumerable contract

Links: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L47
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L81 

Summary:

The `ERC1155Enumerable` contract has an issue in the `_beforeTokenTransfer` and `_afterTokenTransfer` functions where the index variable `i` is not initialized before the for loop, leading to an off-by-one error. This can result in unintended token transfer or missing enumeration.

Impact:

This issue can lead to tokens being transferred incorrectly or token enumeration being incomplete. This can result in unintended token transfers and incorrect balance calculations for users.

Recommendation:

To fix the issue, initialize the `i` variable to `0` before the for loop in the `_beforeTokenTransfer` and `_afterTokenTransfer` functions.

Example:

```
function _beforeTokenTransfer(address, address from, address to, uint256[] memory ids, uint256[] memory amounts, bytes memory) internal virtual override {
    for (uint256 i = 0; i < ids.length; i++) {
        if (amounts[i] != 0) _addTokenEnumeration(from, to, ids[i], amounts[i]);
    }
}
```

and

```
function _afterTokenTransfer(address, address from, address to, uint256[] memory ids, uint256[] memory amounts, bytes memory) internal virtual override {
    for (uint256 i = 0; i < ids.length; i++) {
        if (amounts[i] != 0) _removeTokenEnumeration(from, to, ids[i], amounts[i]);
    }
}
```

