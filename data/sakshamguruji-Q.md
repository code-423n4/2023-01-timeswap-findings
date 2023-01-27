## _addTokenToOwnerEnumeration DOES NOT UPDATE THE allTokens ARRAY

### Description:

The function here https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L116
does not update the _allTokens[] array whereas it should if we are adding a non existential (not present in the allTokens)token to the owner's list
of tokens. 

## MISSING DOCSTRINGS

### Description:

Many functions in the code base lack documentation. This hinders reviewers’ understanding of the code’s intention,
which is fundamental to correctly assess not only security, but also correctness. Additionally, docstrings improve
readability and ease maintenance. They should explicitly explain the purpose or intention of the functions,
the scenarios under which they can fail, the roles allowed to call them, the values returned and the events emitted.

Consider thoroughly documenting all functions (and their parameters) that are part of the contracts’ public API.
Functions implementing sensitive functionality, even if not public, should be clearly documented as well.
When writing docstrings, consider following the Ethereum Natural Specification Format (NatSpec).


For example here in this function https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L85 no comments have been provided


## WRONG ERROR EMITTED

### Description:

Here in this line https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63 we emit the error which should be emitted when there's a zero address , however in this case we are checking that it should not be the zero address.

## WRONG COMMENT IN THE MINT FUNCTION

### Description:

Here in this comment https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L301 it says `Update the fee growth and fees of caller.` while we are not updating the caller but the address `to`.

Similarly there are wrong comments here,

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L145
Instead of `mint logic` it should be `swap logic`

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L183
Again it should be `collect logic` not `mint logic`

## TYPO

### Description:

Maturities instead of maturies here https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L47

## LACK OF 0 ADDRESS CHECKS

### Description:

In the below instances there is a lack of zero address checks , these can lead to permanent loss of funds when not done in functions like mint , transfer etc and assign critical roles to the 0 address.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37 (might assign 0 values to protocol fee and transaction fee)
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L296 param.to !=0
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L158
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L183
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L41
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L71
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L76 (param.to !=0)
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L88-L91
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L60
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L110
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L70
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L99


