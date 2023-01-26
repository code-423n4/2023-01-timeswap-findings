
QA1. ``transferFeesFrom()`` cannot be used by a user to send fees from himself to another user

 https://github.com/code-423n4/2023-01-timeswap/blame/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L79

We can add the condition ``from == msg.sender`` as well to mitigate this issue:
```
if (from != msg.sender && !isApprovedForAll(from, msg.sender)) revert NotApprovedToTransferFees();
```

QA2. A user might mistakenly provide param.to with the address of the contract and lose fund

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L151
 https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L77

Mitigation: Adding a check ``param.to != address(this)`` to ensure not to lose fund.
Adding the following check ``param.long0To != address(this) && param.long1To != address(this) && param.shortTo != address(this) revert ToThisContract();`` to avoid losing fund. 



QA3.  ``transferFeesFrom()`` does not update ``_feesPositions`` properly

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L86-L88

We need to bring  ``_feesPositions`` up to date by calling ``_updateFeesPositions()`` first before ``burn()`` and ``mint()``, therefore, we need to move ``_updateFeesPositions()`` before ``burn()`` and ``mint()``. Otherwise, the fees will be calculated wrongly. 

```
 _updateFeesPositions(from, to, id);          // @audit: updating fees must occur before the changes of liquidities

_feesPositions[id][to].mint(long0Fees, long1Fees, shortFees);

_feesPositions[id][from].burn(long0Fees, long1Fees, shortFees);

```

QA4. ``_removeTokenFromOwnerEnumeration`` REPLACES the wrong ``lastTokenId``.

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


QA5: ``create()`` fails to push the new ``optionPair`` into array ``getByIndex``, as a result, function `` numberOfPairs()`` will return the wrong value.  

``create()`` fails to push the new ``optionPair`` into array ``getByIndex``, as a result, function `` numberOfPairs()`` will return the wrong value.  

The revision is as follows.
```
function create(address token0, address token1) external override returns (address optionPair) {
        if (token0 == address(0)) Error.zeroAddress();
        if (token1 == address(0)) Error.zeroAddress();
        OptionPairLibrary.checkCorrectFormat(token0, token1);

        optionPair = optionPairs[token0][token1];
        OptionPairLibrary.checkDoesNotExist(token0, token1, optionPair);

        optionPair = deploy(address(this), token0, token1);

        optionPairs[token0][token1] = optionPair;

        getByIndex.push(optionPair);   // push the new ``optionPair`` into array ``getByIndex``

        emit Create(msg.sender, token0, token1, optionPair);
    }

```

