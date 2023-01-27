hi , i want to report a QA on a code at TimeswapV2Pool.sol 
Line 82
```
function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {
        return uint96(block.timestamp + durationForward); // truncation is desired
    }
```
i saw that you call this function every time and pass 0 blockTimestamp(0)

so you don't need to call this function every time and pass 0 

```
function blockTimestamp() internal view virtual returns (uint96) {
        return uint96(block.timestamp ); // truncation is desired
    }
```
it can save some bytecode size and deployment gas cost .
