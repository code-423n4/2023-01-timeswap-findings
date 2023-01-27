# Before All
I suggest a higher level of solidity (0.8.17)

# v2-pool

## enums

1. in line: 53,58,63,68,73:
   `uint256(transaction) >= 4` should be replaced with `uint256(transaction) > 3` to save gas.

## TimeswapV2Pool
1. in line 52, 53, 55:
   the use of `private` is unnecessary as no contract inherits this contract. If the dev want this not be seen, can use `internal` instead. But still people can read its value through `web3.eth.getStorageAt()`

## struct

### Pool.sol
1. in line 183: should check if `address to` is `address(0)`.

## TimeswapV2PoolDeployer

1. in line 28: `poolPair = address(new TimeswapV2Pool{salt: keccak256(abi.encode(optionPair))}());`
   The salt value should contain some feature about an tx. Like in Tarot Protocol Deployer, they add msg.sender as one of the factor of salt too. This helps if any deployment goes wrong and want to add another pool.
