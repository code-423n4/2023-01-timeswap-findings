
## Debug information is not deleted

console.log debug statements are not removed


```solidity
File: packages/v2-token/src/TimeswapV2Token.sol

console.log("reaches right before mint in timeswapv2Tokne::mint");//@audit 调试信息没删除
            _mint(param.long0To, id, (param.long0Amount), bytes(""));

```
