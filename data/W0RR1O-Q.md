Unchecked Integer Overflow/Underflow in `unsafeAdd()` function
===========================================
Problematic function:
* https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L14-L18

The `unsafeAdd()` function which was designed to add two uint256 variables without any proper overflow checks. The lack of these checks could lead to unexpected results an potential security vulnerabilities in other contracts such ass `FeeCalculation.sol`.

POC:
I created a `Test` contrat below to demonstrate the vulnerability:
```
pragma solidity =0.8.8;


contract Test {
   function unsafeAdd(uint256 addend1, uint256 addend2) internal pure returns (uint256 sum) {
        unchecked {
            sum = addend1 + addend2;
        }
    }

    function testOverflow() public view returns (uint256) {
        uint256 largeNumber1 = 115792089237316195423570985008687907853269984665640564039457584007913129639935;
        uint256 largeNumber2 = 115792089237316195423570985008687907853269984665640564039457584007913129639935;
        return unsafeAdd(largeNumber1, largeNumber2);
    }
}


``` 
In the `Test` contract above a function `testOverflow()` was created with variables `largeNumber1` and `largeNumber2` are set to the value of `2**256-1` or `115792089237316195423570985008687907853269984665640564039457584007913129639935`. Integer overflows occurs in the `unsafeAdd()` function when it is called and causing the output to be `115792089237316195423570985008687907853269984665640564039457584007913129639935` instead of the expected output which would be `231584078474632390487141970017375815705399969331281128078915168015826259279870` .

```
Expected output = 231584078474632390487141970017375815705399969331281128078915168015826259279870

Actual output = 231584078474632390487141970017375815705399969331281128078915168015826259279870
```

Recommendations:
* It is highly recomended to use SafeMath library to prevent overflow and underflow which could lead to potential vulnerabilities.
