## [N-01] Constructor of ERC1155 contract requires URI as an argument. 

Currently contracts passes "Timeswap V2 uint160 address" and "Timeswap V2 address" for tokens as an URI argument. It is better to provide specific uri as described in openzeppelin docs (https://token-cdn-domain/{id}.json), as it may allow further gamification of tokens.

[TimeswapV2LiquidityToken.sol#L36](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36)
[TimeswapV2Token.sol#L41](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L41)


---


## [N-02] Name of tokens for Etherscan Token Tracker and other explorers.
For the ERC1155 name and symbol parameters are optional unlike ERC721, however, it would be more user-friendly to set these variables explicitly. 

Like:

    string public name = "My token name";
    string public symbol = "mysymbol";
