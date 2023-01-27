## Gas Optimization

### 1. Don't Initialize Variables with Default Value

#### Findings:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/src/test.sol#L54 
 ``` 
 bool globalFailed = false;
 ```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/src/test.sol#L72 
 ``` 
 uint256 hevmCodeSize = 0;
 ```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/src/test.sol#L479 
 ``` 
 for (uint i = 0; i < a.length; i++) {
 ```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/src/test.sol#L54 
 ``` 
 bool globalFailed = false;
 ```
 ### 2. Cache Array Length Outside of Loop

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/demo/demo.sol#L2 
 ``` 
 pragma solidity >=0.5.0;
 ```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/demo/demo.sol#L2 
 ``` 
 pragma solidity >=0.5.0;
 ```
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/lib/forge-std/lib/ds-test/src/test.sol#L16 
 ``` 
 pragma solidity >=0.5.0;
 ```