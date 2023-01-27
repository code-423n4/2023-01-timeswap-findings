## Caching `_feesPosition[id]`

Caching `_feesPosition[id]` into a variable will result in some gas savings by not having to look it up in storage twice in the same function.

``` solidity
        if (long0Fees != 0 || long1Fees != 0 || shortFees != 0) _addTokenEnumeration(from, to, id, 0);
        // add/mint the fees for the new user
        _feesPositions[id][to].mint(long0Fees, long1Fees, shortFees);

        _updateFeesPositions(from, to, id);

        // remove/burn the fees
        _feesPositions[id][from].burn(long0Fees, long1Fees, shortFees);

        if (long0Fees != 0 || long1Fees != 0 || shortFees != 0) _removeTokenEnumeration(from, to, id, 0);

        emit TransferFees(from, to, position, long0Fees, long1Fees, shortFees);
```

## Remediation Steps

Set `_feesPosition[id]` to a variable to avoid having to look it up multiple times in the same function.

## X = X + Y IS MORE EFFICIENT, THAN X  += Y

In `FeesPosition.sol` there are a few instances where += is being used. Changing this to X = X + Y can result in gas savings.

`function update(FeesPosition storage feesPosition, uint160 liquidity, uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth)`

`function getFees(
        FeesPosition storage feesPosition,
        uint256 long0FeesDesired,
        uint256 long1FeesDesired,
        uint256 shortFeesDesired
    ) `

`function mint(FeesPosition storage feesPosition, uint256 long0Fees, uint256 long1Fees, uint256 shortFees)`

`function burn(FeesPosition storage feesPosition, uint256 long0Fees, uint256 long1Fees, uint256 shortFees)`

## Locking Pragma

In `Position.sol` the Pragma statement is using a flag.

``` solidity
pragma solidity ^0.8.8; 
```

Contracts should be deployed with the same compiler version and flags that they have been tested the most with. Locking the pragma helps ensure that contracts do not accidentally get deployed using, for example, the latest compiler which may have higher risks of undiscovered bugs.

## 

