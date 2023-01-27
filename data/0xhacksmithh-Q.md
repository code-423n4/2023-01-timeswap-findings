### [Low-01] Old Solidity Version Used
- Use a solidity version of at least 0.8.13 to get the ability to use `using for`
 with a list of free functions
- Use a solidity version of at least 0.8.12 to get `string.concat()`
 instead of `abi.encodePacked(<str>,<str>)`

*Instances(All solidity files)*

Should use recent, stable, updated version of solidity to remain secure from other versions related bugs, and able to get updated versions' features.