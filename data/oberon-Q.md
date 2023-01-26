## LOW   
---
### LACK OF ADDRESS(0) CHECKS  
The pending owner has a zero address check but the constructor doesn't. Not checking whether the given address is valid can cause issues. Please add zero address checks to the constructor to prevent human errors.
[OwnableTwoSteps.sol:18](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L18)  
##
---
## QA   
---
### OUTDATED COMPILER  
Version 0.8.8 of Solidity is missing key features and bugfixes as listed here:
https://github.com/ethereum/solidity/blob/develop/Changelog.md
Please use Solidity 0.8.17 to remediate.    
  
---
### SPELLING ERRORS
Spelling errors give off an unprofessional image, so I recommend fixing them even if they don't break anything.
  
[TimeswapV2Pool.sol/ITimeswapV2Pool.sol/Param.sol/Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/7cb8c18ba6d0871033ddf518dd79c4444e2f89cc/packages/v2-pool/src/TimeswapV2Pool.sol) many instances of receipient/receipients -> recipient/recipients
[TimeswapV2Pool.sol/ITimeswapV2Option.sol](https://github.com/code-423n4/2023-01-timeswap/blob/7cb8c18ba6d0871033ddf518dd79c4444e2f89cc/packages/v2-pool/src/TimeswapV2Pool.sol#L81) overidden -> overridden
[TimeswapV2Option.sol:31:47](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L31) maturies -> maturities
[TimeswapV2Token.sol:109](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L109) Tokne -> Token
[Pool.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L179) transferrred -> transferred
[ConstantProduct.sol/ITimeswapV2LiquidityToken.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L19) liqudity -> liquidity
[ConstantProduct.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/ConstantProduct.sol#L394) disriminant -> discriminant
[PoolFactory.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolFactory.sol#L27) retreived -> retrieved
[PoolPair.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/PoolPair.sol#L19) doesn -> doesn't
[FullMath.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L40) signficant -> significant
[FullMath.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/FullMath.sol#L250) precoditions -> preconditions
[StrikeConversion.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol#L40) toekn1 -> token1
[CallbackParam.sol/Param.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L4) paramater -> parameter
[CallbackParam.sol/Param.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol#L32) initalize -> initialize
[ITimeswapV2Token.sol](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/ITimeswapV2Token.sol#L27) postion -> position
  
---
### CHARACTERS PER LINE EXCEEDS 120
[Solidity documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html?highlight=120#maximum-line-length) recommends a maximum of 120 characters per line for readability. Currently some param lines such as [TimeswapV2Pool.sol:128](https://github.com/code-423n4/2023-01-timeswap/blob/7cb8c18ba6d0871033ddf518dd79c4444e2f89cc/packages/v2-pool/src/TimeswapV2Pool.sol#L128) go over 170 characters which makes readability awful.
This can be easily rectified using [forge formatting](https://book.getfoundry.sh/reference/config/formatter?highlight=fmt#formatter).