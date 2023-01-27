### [L-01] `Option.transferPosition()` works with any `amount` when `msg.sender == to`

- https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Option.sol#L64-L66

```solidity
    function transferPosition(Option storage option, address to, TimeswapV2OptionPosition position, uint256 amount) internal {
        if (position == TimeswapV2OptionPosition.Long0) {
            option.long0[msg.sender] -= amount;
            option.long0[to] += amount;
        } else if (position == TimeswapV2OptionPosition.Long1) {
            option.long1[to] += amount; //@audit should minus first
            option.long1[msg.sender] -= amount;
        } else if (position == TimeswapV2OptionPosition.Short) {
            option.short[msg.sender] -= amount;
            option.short[to] += amount;
        }
    }
```

Currently, it adds amount first for the `TimeswapV2OptionPosition.Long1` position so the function will work properly with any `amount` if `msg.sender == to`.

So [TimeswapV2Option.transferPosition()](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L105) will emit a wrong event.


### [L-02] Lack of checks-effects-interactions

- https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L190-L192
- https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2Option.sol#L277

It's recommended to execute external calls after state changes, to prevent reetrancy bugs.


### [L-03] Wrong comments
- https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/structs/Process.sol#L32

```solidity
/// @param isAddToken1 IsAddToken1 if true. IsSubToken0 if false.
```

`IsSubToken0` should be `IsSubToken1`.


### [N-01] Incorrect event name
- https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L31

```solidity
emit SetOwner(pendingOwner);
```

It should be `SetPendingOwner`, otherwise, it might make users confusing.