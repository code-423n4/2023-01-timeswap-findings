Combining the overflow and out-of-bounds checks in a single `require()` statement
=====================================================
problematic code:
* https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#L13-L14

In the `slice()` function the `require()` statement is checking two conditions which is overflow and out-of-bounds. By combining these two checks into a single `require()` statement this helps the number of time `require()` checks are being performed and helps to save gas.

Here is an example to fix this issue:
```
pragma solidity =0.8.8;

contract test1{
    function slice(bytes memory _bytes, uint256 _start, uint256 _length) internal pure returns (bytes memory) {
        require((_length + 31 >= _length) && (_bytes.length >= _start + _length) , "slice_overflow or slice_outOfBounds");
        // rest of the code
    }
}


```
 

