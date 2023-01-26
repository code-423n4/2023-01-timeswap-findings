# Summary:
The codebase is very solid and doesn't present any particular security risks. Tests are a bit lacking and incomplete, but they are a work in progress. It does not pose any concerning centralization risks.


# Low Risk findings:

## [LC] `numberOfPairs` is not implemented and always returns 0

In `TimeswapV2OptionFactory.sol` `getByIndex` is never assigned, so `numberOfPairs` has always zero length.

Code:
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L52-L54
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L33

## [LC] It's possible to initialize a pool with a strike of 0

In `TimeswapV2OptionFactory.sol` there isn't a check for creating a pool with a strike of zero. This is low risk because everywhere else (mint, burn, leverage) there is a check, so these transactions will fail.

Code:
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L175-L181

## [LC] Pool functions with a duration forward will always revert

If mint, burn, leverage, or deleverage are called with a `durationForward`, the `isQuote` parameter will always be true, thus reverting the transaction. These functions will never go through, so it would make sense to remove them entirely.

Example with mint:

```solidity
function mint(
        TimeswapV2PoolMintParam calldata param,
        uint96 durationForward
    ) external override returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
        return mint(param, true, durationForward);
}

function mint(
        TimeswapV2PoolMintParam calldata param,
        bool isQuote,
        uint96 durationForward
    ) private noDelegateCall returns (uint160 liquidityAmount, uint256 long0Amount, uint256 long1Amount, uint256 shortAmount, bytes memory data) {
       ...
     if (isQuote) revert Quote();
       ...
}
```

Code:
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L245-L250
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L310-L315
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L370-L375
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L426-L428

# Non critical findings

## [NC] When creating a pool with an existing address, the error name should be `notZeroAddress`
The second `zeroAddress` revert is a bit misleading in this case because the transaction will fail if the address is *not* the zero address. This is confusing because before that it was used in the opposite way:

```
if (optionPair == address(0)) Error.zeroAddress();
pair = pairs[optionPair];
if (pair != address(0)) Error.zeroAddress();
```

Code:
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L59-L71

## [NC] Grammar error/typos in docs

1) Should be maturities instead of *maturies*.
`/// @dev mapping of all option state for all strikes and maturies.` 

Code:
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L31
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L47

2) Missing @param maturity
```solidity
/// @dev Reverts when an option of given strike and maturity is still inactive.
    /// @param strike The chosen strike.
    function inactiveOptionChoice(uint256 strike, uint256 maturity) external pure {
        return Error.inactiveOptionChoice(strike, maturity);
    }
```
Code:
https://github.com/code-423n4/2023-01-timeswap/blob/7cb8c18ba6d0871033ddf518dd79c4444e2f89cc/packages/v2-library/test/wrapped/ErrorExt.sol#L64-L68



