Hi , 

there is Incorrect Calculation in TimeswapV2Pool.sol line 82

```
function blockTimestamp(uint96 durationForward) internal view virtual returns (uint96) {
        return uint96(block.timestamp + durationForward); // here 
    }

```
line 267
```
if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;
```
line 271
```
if (long1Amount != 0) long1BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long1) + long1Amount;
```
line 274 
```
uint256 shortBalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Short) + shortAmount;
```
line 393 
```
 if (long0Amount != 0) long0BalanceTarget = ITimeswapV2Option(optionPair).positionOf(param.strike, param.maturity, address(this), TimeswapV2OptionPosition.Long0) + long0Amount;
```
line 397
```
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
lead to Integer Overflow and Underflow

* Remediation

It is recommended to use vetted safe math libraries for arithmetic operations consistently throughout the smart contract system.

* References 

https://swcregistry.io/docs/SWC-101#tokensalechallengesol 

https://cwe.mitre.org/data/definitions/682.html