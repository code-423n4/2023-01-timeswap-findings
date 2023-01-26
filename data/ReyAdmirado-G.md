| | issue |
| ----------- | ----------- |
| 1 | [Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate](#1-multiple-addressid-mappings-can-be-combined-into-a-single-mapping-of-an-addressid-to-a-struct-where-appropriate) |
| 2 | [state variables should be cached in stack variables rather than re-reading them from storage (ones that were not mentioned in c4udit)](#2-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage-ones-that-were-not-mentioned-in-c4udit) |
| 3 | [`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables](#3-x--y-costs-more-gas-than-x--x--y-for-state-variables) |
| 4 | [not using the named return variables when a function returns, wastes deployment gas](#4-not-using-the-named-return-variables-when-a-function-returns-wastes-deployment-gas) |
| 5 | [can make the variable outside the loop to save gas](#5-can-make-the-variable-outside-the-loop-to-save-gas) |
| 6 | [use a more recent version of solidity](#6-use-a-more-recent-version-of-solidity) |
| 7 | [usage of uint/int smaller than 32 bytes (256 bits) incurs overhead](#7-usage-of-uintint-smaller-than-32-bytes-256-bits-incurs-overhead) |
| 8 | [switch `if check` positions](#8-switch-if-check-positions) |


## 1. Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. 

`_timeswapV2TokenPositions` and `_timeswapV2TokenPositionIds` are used in the same function a bunch of times
- [TimeswapV2Token.sol#L38](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L38)
- [TimeswapV2Token.sol#L39](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L39)


## 2. state variables should be cached in stack variables rather than re-reading them from storage (ones that were not mentioned in c4udit)

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. 

`liquidityPosition.long0Fees` is being read once inside the if check and if it passes its gonna be read again inside the if statement.if the condition fails its gonna be read inside the else statement instead. so we can cache the `liquidityPosition.long0Fees` variable before the if condition so it can be read one less time.
- [LiquidityPosition.sol#L91-L97](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L91-L97)

`liquidityPosition.long1Fees` is being read once inside the if check and if it passes its gonna be read again inside the if statement.if the condition fails its gonna be read inside the else statement instead. so we can cache the `liquidityPosition.long1Fees` variable before the if condition so it can be read one less time.
- [LiquidityPosition.sol#L99-L105](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L99-L105)

`liquidityPosition.shortFees` is being read once inside the if check and if it passes its gonna be read again inside the if statement.if the condition fails its gonna be read inside the else statement instead. so we can cache the `liquidityPosition.shortFees` variable before the if condition so it can be read one less time.
- [LiquidityPosition.sol#L107-L112](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L107-L112)

`pool.liquidity` is being read twice. cache it at start of the function to save one extra read from storage
- [Pool.sol#L102-L103](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L102-L103)

`pool.sqrtInterestRate` is being read twice. cache it at start of the function to save one extra read from storage
- [Pool.sol#L102-L103](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L102-L103)

because usually the if statement will be true we can cache `pool.liquidity` and save lots of gas with a low risk of using only 3 more gas
- [Pool.sol#L206](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L206)

`pool.long0ProtocolFees` is being read once inside the if check and if it passes its gonna be read again inside the if statement.if the condition fails its gonna be read inside the else statement instead. so we can cache the `pool.long0ProtocolFees` variable before the if condition so it can be read one less time.
- [Pool.sol#L226-L232](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L226-L232)

`pool.long1ProtocolFees` is being read once inside the if check and if it passes its gonna be read again inside the if statement.if the condition fails its gonna be read inside the else statement instead. so we can cache the `pool.long1ProtocolFees` variable before the if condition so it can be read one less time.
- [Pool.sol#L234-240](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L234-240)

`pool.shortProtocolFees` is being read once inside the if check and if it passes its gonna be read again inside the if statement.if the condition fails its gonna be read inside the else statement instead. so we can cache the `pool.shortProtocolFees` variable before the if condition so it can be read one less time.
- [Pool.sol#L242-L247](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L242-L247)

`pool.liquidity` is read twice in both if checks at lines L271 and L279
- [Pool.sol#L271](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L271)

`pool.liquidity` is being read three times. cache it at the start of the function. read once in L559 once in one of the if or else statements below that and once in L530. cache it to save 2 extra storage read
- [Pool.sol#L476](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L476)

`pool.liquidity` is being read twice and highly possibly 4 times cache it at the start of the function to save lots of gas.read once in L559 once in one of the if or else statements below that and highly possibly gonna be read in L639 and L653
- [Pool.sol#L559](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L559)

`pool.liquidity` is being read twice. L666 and once of the L697 or L724
- [Pool.sol#L666](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L666)

`option.totalLong0` is being read twice. firstly in L192 and in one of the if or elses below it. cache it to have one less storage read(also theres is -= which is wasting gas and is pointed out at another point of the report. after changing that there is gonna be one more storage read that can be prevented)
- [Option.sol#L192](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L192)

`option.totalLong1` is being read twice. firstly in L192 and in one of the if or elses below it. cache it to have one less storage read.(also theres is -= which is wasting gas and is pointed out at another point of the report. after changing that there is gonna be one more storage read that can be prevented)
- [Option.sol#L193](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L193)

`_feesPositions[id][msg.sender]` is being read twice. cache it before L199 and use the functions on the cached version of it on L199 and L216
- [TimeswapV2LiquidityToken.sol#L199](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L199)


## 3. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
Using the addition operator instead of plus-equals saves gas

- [LiquidityPosition.sol#L55](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L55)
- [LiquidityPosition.sol#L56](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L56)
- [LiquidityPosition.sol#L57](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L57)
- [LiquidityPosition.sol#L66](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L66)
- [LiquidityPosition.sol#L70](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L70)
- [LiquidityPosition.sol#L71](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L71)
- [LiquidityPosition.sol#L72](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L72)
- [LiquidityPosition.sol#L76](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L76)
- [LiquidityPosition.sol#L80](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L80)
- [LiquidityPosition.sol#L81](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L81)
- [LiquidityPosition.sol#L82](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/LiquidityPosition.sol#L82)

- [Pool.sol#L361](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L361)
- [Pool.sol#L362](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L362)
- [Pool.sol#L365](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L365)
- [Pool.sol#L537](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L537)
- [Pool.sol#L538](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L538)
- [Pool.sol#L695](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L695)
- [Pool.sol#L722](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L722)
- [Pool.sol#L452](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L452)
- [Pool.sol#L453](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L453)
- [Pool.sol#L455](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L455)
- [Pool.sol#L636](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L636)
- [Pool.sol#L650](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L650)
- [Pool.sol#L677](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L677)
- [Pool.sol#L689](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L689)
- [Pool.sol#L710](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L710)
- [Pool.sol#L719](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L719)

- [Option.sol#L103](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L103)
- [Option.sol#L108](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L108)
- [Option.sol#L174](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L174)
- [Option.sol#L178](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L178)
- [Option.sol#L141](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L141)
- [Option.sol#L142](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L142)
- [Option.sol#L173](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L173)
- [Option.sol#L177](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L177)
- [Option.sol#L208](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L208)
- [Option.sol#L209](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol#L209)

- [FeesPosition.sol#L59](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L59)
- [FeesPosition.sol#L60](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L60)
- [FeesPosition.sol#L61](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L61)
- [FeesPosition.sol#L31](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L31)
- [FeesPosition.sol#L32](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L32)
- [FeesPosition.sol#L33](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L33)
- [FeesPosition.sol#L53](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L53)
- [FeesPosition.sol#L54](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L54)
- [FeesPosition.sol#L55](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/FeesPosition.sol#L55)


## 4. not using the named return variables when a function returns, wastes deployment gas

the variables declared are never used so there is no need of declaring them
- [Pool.sol#L66](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L66)
- [Pool.sol#L83](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol#L83)


## 5. can make the variable outside the loop to save gas

make the variable outside the loop and only give the value to variable inside

- [Process.sol#L35](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Process.sol#L35)


## 6. use a more recent version of solidity

Use a solidity version of at least 0.8.10 to have `external` calls skip contract existence checks if the external call has a return value
Use a solidity version of at least 0.8.12 to get string.concat() to be used instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions


## 7. usage of uint/int smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed


## 8. switch `if check` positions 

the if check used on the first checks uses higher gas, so we can swap the positions and sort them by gas usage so if a cheaper one will return error for us we dont have to check the other expensive ones

switch the position of the first `if` to last because its the most expensive
- [TimeswapV2Option.sol#L98-L100](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2Option.sol#L98-L100)

bring the first to last because it uses a function
- [TimeswapV2Pool.sol#L155-157](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L155-157)

bring the first to last because it uses a function
- [TimeswapV2Pool.sol#L176-177](https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L176-177)
