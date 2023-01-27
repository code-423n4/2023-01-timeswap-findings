hi , 

it's seems that you don't use Unchecked Math ! like in the code at TimeswapV2Pool.sol 
line 82
```
  function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {
        return uint96(block.timestamp + durationForward); // truncation is desired
    }
```
line 266
```
 uint256 long0BalanceTarget;
        if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;

        // long1Amount chosen could be zero. Skip the calculation for gas efficiency.
        uint256 long1BalanceTarget;
        if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;

        // shortAmount cannot be zero.
        uint256 shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;

```
line 392
```
        uint256 long0BalanceTarget;
        if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;

        // long1Amount chosen could be zero. Skip the calculation for gas efficiency.
        uint256 long1BalanceTarget;
        if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;
```
line 444
```
        uint256 balanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;
```
line 479
```
uint256 balanceTarget = ITimeswapV2Option(optionPair).positionOf(
            param.strike,
            param.maturity,
            address(this),
            param.isLong0ToLong1 ? TimeswapV2OptionPosition.Long0 : TimeswapV2OptionPosition.Long1
        ) + (param.isLong0ToLong1 ? long0Amount : long1Amount);
```
Disabling overflow / underflow check saves gas. 
