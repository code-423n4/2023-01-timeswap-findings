# Lack of input validitation in several functions within Math.sol Library
 - The following functions unsafeAdd(), unsafeSub(), div(), min() do not have a valid input validation check when performing arithmetic operations.
 - https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol
```
function unsafeAdd(uint256 addend1, uint256 addend2) internal pure returns (uint256 sum) {
        unchecked {
            // @audit if sum < addend1 revert error Overflow();
            sum = addend1 + addend2;
        }
    }

function unsafeSub(uint256 minuend, uint256 subtrahend) internal pure returns (uint256 difference) {
        unchecked {
            // @audit if(subtrahend > minuend) revert Underflow(); 
            difference = minuend - subtrahend;
        }
    }

function min(uint256 value1, uint256 value2) internal pure returns (uint256 result) {
        //@audit if(value1 == 0 && value2 == 0) revert ValueIsZero();
        return value1 < value2 ? value1 : value2;
    }

```
# Inappropriate name for certain functions
 - The naming of the function "unsafe" does not align with best practices. Consider renaming any functions that begin with "unsafe" to a more appropriate/safer name.
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol
- https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/DurationCalculation.sol

# Non-unique functions name for mint/burn
- Consider using unique names for mint/burn functions in TimeSwapV2Pool instead of having 3 functions with the same name within the same contract. This will prevent confusion when reading the code, make it easier for the team when implementing the functions with the front-end, and make it clearer for users when interacting with the protocol through Etherscan/Polygonscan.

# Lack of input validation for zero address or 0 input (Bonus)
- I know that checking zero addresses, and the input value is !=0 are no longer rewarded since they are considered publicly known issues, however, the protocol is not implementing these checks in most of its functions. This decreases the security of its code and opens up more vulnerabilities. Please consider adding proper input checks in functions and constructors.