1.  Using Pure instead of View

The check function here should be view instead of pure. In pure function, no state variable can be changed or read but in the code below it read from the state variables NOT_INTERACTED and ENTERED. View function is more appropriate to be used as it can read from state variables. 

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/ReentrancyGuard.sol#L23

