## [L-01] USING VULNERABLE DEPENDENCY OF OPENZEPPELIN
    
[packages/v2-library/package.json#40](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/package.json#40)
The package.json configuration file says that the project is using 4.7.2 of OZ which has a not last update version
package.json
```

VULNERABILITY	VULNERABLE VERSION
H    	Improper Verification of Cryptographic Signature	<4.7.3
M   	Denial of Service (DoS)	>=2.3.0 <4.7.2
L     	Incorrect Resource Transfer Between Spheres	 >=4.6.0 <4.7.2
H	Incorrect Calculation	>=4.3.0 <4.7.2
H	Information Exposure	>=4.1.0 <4.7.1
H	Information Exposure	>=4.0.0 <4.7.1

```
### Recommendation:
Use patched versions
Latest non vulnerable version 4.8.0       

                                                                                
                                        
## [N-01] INCLUDE RETURN PARAMETERS IN NATSPEC COMMENTS
[packages/v2-library/src/BytesLib.sol#12](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/BytesLib.sol#12)
[packages/v2-library/src/CatchError.sol#12](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/CatchError.sol#12)
[packages/v2-library/src/StrikeConversion.sol
](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/StrikeConversion.sol)
[packages/v2-token/src/structs/FeesPosition.sol](https://github.com/code-423n4/2023-01-timeswap/tree/main/packages/v2-token/src/structs)

## [N-02] USE A MORE RECENT VERSION OF SOLIDITY
### Context:
All contracts

### Description:
For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md

### Recommendation:
Old version of Solidity is used , newer version can be used (0.8.17)
## [N-03] SOLIDITY COMPILER OPTIMIZATIONS CAN BE PROBLEMATIC
[packages/v2-pool/hardhat.config.ts#L34](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/hardhat.config.ts#L34)
[packages/v2-option/hardhat.config.ts#L35](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/hardhat.config.ts#L35)
[packages/v2-token/hardhat.config.ts#L34](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/hardhat.config.ts#34)

## [N-03] OMISSIONS IN EVENTS
Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

The events should include the new value and old value where possible:

[packages/v2-token/src/interfaces/ITimeswapV2LiquidityToken.sol#L236 ](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L236 )                                       [packages/v2-token/src/structs/FeesPosition.sol #L29    ](  
 https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L29       )             