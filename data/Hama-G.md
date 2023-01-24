 title:
abi.encode() is less efficient than abi.encodePacked()

links:
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolDeployer.sol#L28
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol#L35

mitigation: 
        optionPair = address(new TimeswapV2Option{salt: keccak256(abi.encodePacked(token0, token1))}());
