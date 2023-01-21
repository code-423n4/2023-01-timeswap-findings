# Gas Optimizations Report

|         | Issue                                                                                                              | Instances |
| ------- | :----------------------------------------------------------------------------------------------------------------- | :-------: |
| [G-001] | x += y or x -= y costs more gas than x = x + y or x = x - y for state variables                                    |     5     |
| [G-002] | Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate |     2     |
| [G-003] | internal functions only called once can be inlined to save gas                                                     |     7     |
| [G-004] | Replace modifier with function                                                                                     |     2     |

## [G-001] x += y or x -= y costs more gas than x = x + y or x = x - y for state variables

### Impact

Using the addition operator instead of plus-equals saves 113 gas. Usually does not work with struct and mappings.

### Findings

Total:5

[packages/v2-pool/src/libraries/ConstantProduct.sol#L150](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-pool/src/libraries/ConstantProduct.sol#L150)

```solidity
150:    else shortAmount -= fees;
```

[packages/v2-pool/src/libraries/ConstantProduct.sol#L244](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-pool/src/libraries/ConstantProduct.sol#L244)

```solidity
244:    amount -= fees;
```

[packages/v2-pool/src/libraries/ConstantProduct.sol#L180](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-pool/src/libraries/ConstantProduct.sol#L180)

```solidity
180:    shortAmount -= fees;
```

[packages/v2-pool/src/libraries/ConstantProduct.sol#L149](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-pool/src/libraries/ConstantProduct.sol#L149)

```solidity
149:    if (isAdd) longAmount -= fees;
```

[packages/v2-pool/src/libraries/ConstantProduct.sol#L212](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-pool/src/libraries/ConstantProduct.sol#L212)

```solidity
212:    longAmount -= fees;
```

## [G-002] Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

### Impact

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key's keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

### Findings

Total:2

[packages/v2-option/src/structs/Option.sol#L23-L25](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-option/src/structs/Option.sol#L23-L25)

```solidity
23:    mapping(address => uint256) long0;
24:        mapping(address => uint256) long1;
25:        mapping(address => uint256) short;
```

[packages/v2-token/src/base/ERC1155Enumerable.sol#L17-L20](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-token/src/base/ERC1155Enumerable.sol#L17-L20)

```solidity
17:    mapping(address => uint256) private _currentIndex; // the current index for an address
18:
19:        // Mapping from token ID to index of the owner tokens list
20:        mapping(uint256 => uint256) private _ownedTokensIndex;
```

## [G-003] internal functions only called once can be inlined to save gas

### Impact

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.

### Findings

Total:7

[packages/v2-token/src/base/ERC1155Enumerable.sol#L109](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-token/src/base/ERC1155Enumerable.sol#L109)

```solidity
109:    function _additionalConditionRemoveTokenFromOwnerEnumeration(address, uint256) internal virtual returns (bool) {
```

[packages/v2-token/src/base/ERC1155Enumerable.sol#L92](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-token/src/base/ERC1155Enumerable.sol#L92)

```solidity
92:    function _removeTokenEnumeration(address from, address to, uint256 id, uint256 amount) internal {
```

[packages/v2-token/src/base/ERC1155Enumerable.sol#L58](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-token/src/base/ERC1155Enumerable.sol#L58)

```solidity
58:    function _addTokenEnumeration(address from, address to, uint256 id, uint256 amount) internal {
```

[packages/v2-token/src/base/ERC1155Enumerable.sol#L70](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-token/src/base/ERC1155Enumerable.sol#L70)

```solidity
70:    function _additionalConditionAddTokenToAllTokensEnumeration(uint256) internal virtual returns (bool) {
```

[packages/v2-token/src/base/ERC1155Enumerable.sol#L75](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-token/src/base/ERC1155Enumerable.sol#L75)

```solidity
75:    function _additionalConditionAddTokenToOwnerEnumeration(address, uint256) internal virtual returns (bool) {
```

[packages/v2-token/src/base/ERC1155Enumerable.sol#L104](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-token/src/base/ERC1155Enumerable.sol#L104)

```solidity
104:    function _additionalConditionRemoveTokenFromAllTokensEnumeration(uint256) internal virtual returns (bool) {
```

[packages/v2-token/src/TimeswapV2LiquidityToken.sol#L224](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-token/src/TimeswapV2LiquidityToken.sol#L224)

```solidity
224:    function _beforeTokenTransfer(address operator, address from, address to, uint256[] memory ids, uint256[] memory amounts, bytes memory data) internal override {
```

## [G-004] Replace modifier with function

### Impact

Modifiers make code more elegant, but cost more than normal functions.

### Findings

Total:2

[packages/v2-option/src/NoDelegateCall.sol#L34](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-option/src/NoDelegateCall.sol#L34)

```solidity
34:    modifier noDelegateCall() {
```

[packages/v2-pool/src/NoDelegateCall.sol#L34](https://github.com/code-423n4/2023-01-timeswap/tree/main//packages/v2-pool/src/NoDelegateCall.sol#L34)

```solidity
34:    modifier noDelegateCall() {
```
