# Reorder struct layout
## The following struct could be optimized moving the position of certain values in order to save slot storage:

TimeswapV2TokenPosition in  [Position.sol#L12-L18](https://github.com/code-423n4/2023-01-timeswap/blob/7cb8c18ba6d0871033ddf518dd79c4444e2f89cc/packages/v2-token/src/structs/Position.sol#L12-L18)

    struct TimeswapV2TokenPosition {
        address token0;
        address token1;
    +    TimeswapV2OptionPosition position;
        uint256 strike;
        uint256 maturity;
    -    TimeswapV2OptionPosition position;
    }

