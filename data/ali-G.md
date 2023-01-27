Replace state variable reads and writes within loops with local variable reads and writes. This is done by assigning state variable values to new local variables, reading and/or writing the local variables in a loop, then after the loop assigning any changed local variables to their equivalent state variables.

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L91

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L92

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L93

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L96

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L99

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L100

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L101

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L104

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L107

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L108

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L109

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/LiquidityPosition.sol#L112

can be fixed like this;

```     
function collectTransactionFees(
        LiquidityPosition storage liquidityPosition,
        uint256 long0Requested,
        uint256 long1Requested,
        uint256 shortRequested
    ) internal returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {
        uint256 l0f = liquidityPosition.long0Fees;
        uint256 l1f = liquidityPosition.long1Fees;
        uint256 sf = liquidityPosition.shortFees;
        if (long0Requested >= l0f) {
            long0Fees = l0f;
            l0f = 0;
        } else {
            long0Fees = long0Requested;
            l0f = l0f;
        }

        if (long1Requested >= l1f) {
            long1Fees = l1f;
            l1f = 0;
        } else {
            long1Fees = long1Requested;
            l1f = l1f;
        }

        if (shortRequested >= sf) {
            shortFees = sf;
            sf = 0;
        } else {
            shortFees = shortRequested;
            sf = sf;
        }
    }
```

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L226

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L227

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L228

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L231

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L234

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L235

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L236

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L239

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L242

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L243

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L244

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/structs/Pool.sol#L247

can be fixed like this;

```     
    function collectProtocolFees(
        Pool storage pool,
        uint256 long0Requested,
        uint256 long1Requested,
        uint256 shortRequested
    ) external returns (uint256 long0Amount, uint256 long1Amount, uint256 shortAmount) {
        uint256 l0pf = pool.long0ProtocolFees;
        uint256 l1pf = pool.long1ProtocolFees;
        uint256 spf = pool.shortProtocolFees;
        if (long0Requested >= l0pf) {
            long0Amount = l0pf;
            l0pf = 0;
        } else {
            long0Amount = long0Requested;
           l0pf = l0pf.unsafeSub(long0Requested);
        }

        if (long1Requested >= l1pf) {
            long1Amount = l1pf;
            l1pf = 0;
        } else {
            long1Amount = long1Requested;
            l1pf = l1pf.unsafeSub(long1Requested);
        }

        if (shortRequested >= spf) {
            shortAmount = spf;
           spf = 0;
        } else {
            shortAmount = shortRequested;
            spf = spf.unsafeSub(shortRequested);
        }
    }
```