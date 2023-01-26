QA1. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L47-L55
We need to make sure the two arrays, ``ids`` and ``amounts``, have the same length.
```
if(ids.length != amounts.length) revert ArrayLengthsNotEqual();

```

QA2. https://github.com/code-423n4/2023-01-timeswap/blame/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L79
We should check if ``from == msg.sender`` as well here
```
if (from != msg.sender && !isApprovedForAll(from, msg.sender)) revert NotApprovedToTransferFees();
```

QA3. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L151
Adding a check ``param.to != address(this)`` to ensure not to lose fund.

QA4. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L86-L88
These two statements need to be exchanged. We need to perform the ``_updateFeesPositions()`` before ``burn()`` and ``mint()``, so that fees can be calculated for both ``from`` and ``to`` with the right amount of liquidity (the original liquidity amount) and brought up to date.
```
 _updateFeesPositions(from, to, id);          // @audit: updating fees must occur before the changes of liquidities

_feesPositions[id][to].mint(long0Fees, long1Fees, shortFees);

_feesPositions[id][from].burn(long0Fees, long1Fees, shortFees);

```

QA5. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L77
Adding the following check ``param.long0To != address(this) && param.long1To != address(this) && param.shortTo != address(this) revert ToThisContract();`` to avoid losing fund. 

QA6. https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L199
Adding the following check ``param.long0To != address(this) && param.long1To != address(this) && param.shortTo != address(this) revert ToThisContract();`` to avoid losing fund. 

QA7. ``_removeTokenFromOwnerEnumeration`` REPLACES the wrong ``lastTokenId``.

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L136-L149

The function removes ``tokenId`` from the list of ``from`` by replacing it with ``lastTokenId``. However the following statement of retrieving the ``lastTokenId`` is wrong:
 
```
 uint256 lastTokenIndex = _currentIndex[from] - 1;
```

This is because  the last  index used is ``_currentIndex[from]``, see the implementation:
https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/base/ERC1155Enumerable.sol#L116

The fix is as follows:
```
function _removeTokenFromOwnerEnumeration(address from, uint256 tokenId) private {
        uint256 lastTokenIndex = _currentIndex[from];   // @audit: fix here
        uint256 tokenIndex = _ownedTokensIndex[tokenId];

        if (tokenIndex != lastTokenIndex) {
            uint256 lastTokenId = _ownedTokens[from][lastTokenIndex];

            _ownedTokens[from][tokenIndex] = lastTokenId;
            _ownedTokensIndex[lastTokenId] = tokenIndex;
        }

        delete _ownedTokensIndex[tokenId];
        delete _ownedTokens[from][lastTokenIndex];
}
```

