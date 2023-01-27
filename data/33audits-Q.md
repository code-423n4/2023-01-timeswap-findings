
In `NoDelegateCall.sol` there needs to be a zero address check in the constructor. Zero-address checks as input validation on address parameters is always a best practice. This is especially true for critical addresses that are immutable and set in the constructor because they cannot be changed later. Accidentally using zero addresses here will lead to failing logic or force contract redeployment and increased gas costs.

``` solidity
constructor() {
        // Immutables are computed in the init code of the contract, and then inlined into the deployed bytecode.
        // In other words, this variable won't change when it's checked at runtime.
        original = address(this);
    }
```

## Recommended Mitigation Steps
Add zero-address input validation for these addresses in the constructor.