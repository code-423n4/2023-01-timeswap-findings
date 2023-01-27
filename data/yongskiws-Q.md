
### [L-01] Functions return bool but cannot return false
Some functions return bool but only return true unless reverting. This means the return value is meaningless. This may cause problems if they are elsewhere expected to return false rather than revert.
``` solidity
token\base\ERC1155Enumerable.sol
71:         return true;
76:         return true;
76:         return true;
110:       return true;
```

### [NC-1] console.log is deprecated 
``` solidity
File: c:\Users\pc\Desktop\code\timeswap\token\TimeswapV2Token.sol
5: import "forge-std/console.sol";
109:            console.log("reaches right before mint in timeswapv2Tokne::mint");
```

### [NC-2] toKey() same function change to improve code readability

``` solidity
token\TimeswapV2LiquidityToken.sol
66: timeswapV2LiquidityTokenPosition.toKey()
token\TimeswapV2Token.sol
72: timeswapV2TokenPosition.toKey()
```

``` solidity
structs\Position.sol
34:     function toKey(TimeswapV2TokenPosition memory timeswapV2TokenPosition) internal pure returns (bytes32) {
35:         return keccak256(abi.encode(timeswapV2TokenPosition));
36:     }
37: 
38:     /// @dev return keccak for key management for Liquidity Token.
39:     function toKey(TimeswapV2LiquidityTokenPosition memory timeswapV2LiquidityTokenPosition) internal pure returns (bytes32) {
40:         return keccak256(abi.encode(timeswapV2LiquidityTokenPosition));
41:     }
```

EG.
``` solidity
function toKey
function toKeyLiq
```