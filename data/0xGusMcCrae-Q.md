### Low Risk Items
| Number |Issues Details|
|:--:|:-------|
|[L-01]| No entry point for EOA accounts |
|[L-02]| Uninitialized state variables |
|[L-03]| Missing zero-checks |
|[L-04]| Optimizer runs can cause errors |

### Non-Critical Items
| Number |Issues Details|
|:--:|:-------|
|[NC-01]| Inconsistent use of Error.zeroAddress |
|[NC-02]| checkDoesNotExist is not implemented |
|[NC-03]| unsafeDurationFromLastTimestampToNowOrMaturity is not implemented |
|[NC-04]| Remaining console.log |


## L-01 - No entry point for EOA accounts 

EOA accounts can't interact with the protocol's main external functions which handle some inbound communication and token transfers via callbacks. 

For example, the `TimeswapV2Pool.mint` function makes 2 callbacks, `timeswapV2PoolMintChoiceCallback` (within the pool struct library), and `timeswapV2PoolMintCallback`. 

Below is an edited version of testLiquidity in TimeswapV2Pool.t.sol so that a random EOA (address endUser) is the caller of `TimeswapV2Pool.mint`. It will always revert because EOAs can't receive/respond to the callback functions.

    function testLiquidity(
            ...
            address endUser
        ) public {
            vm.assume(
                ...
                endUser != address(0)
            );
            ...
            startHoax(endUser);
            setUp();
            pool.initialize(strike, time.maturity, rate);

            TimeswapV2PoolMintParam memory param = TimeswapV2PoolMintParam({strike: strike, maturity: time.maturity, to: to, transaction: TimeswapV2PoolMint.GivenLong, delta: delta, data: ""});
            
            MintOutput memory response;
            console.log("before mint");
            (response.liquidityAmount, response.long0Amount, response.long1Amount, response.shortAmount, response.data) = pool.mint(param);
            console.log("after mint");
            ...

Output:

    ...
    ─ [117453] TimeswapV2Pool::initialize(1, 2, 2980062428665914318173239922533264937601317)
    │   ├─ [24741] PoolLibrary::c3218b1d(58e76cff22dd72278c8f84685a17f449f02ff85d2e9a03f82022b6f395640860000000000000000000000000000022359dcac048524924924924924924924925) [delegatecall]
    │   │   └─ ← ()
    │   └─ ← ()
    ├─ [0] console::log(before mint) [staticcall]
    │   └─ ← ()
    ├─ [8913] TimeswapV2Pool::mint((1, 2, 0x0000000000000000000000000000000000000001, 1, 56, 0x))
    │   ├─ [4828] PoolLibrary::6951fc8c(58e76cff22dd72278c8f84685a17f449f02ff85d2e9a03f82022b6f395640860000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000003800000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000000) [delegatecall]    │   │   ├─ [0] console::log(before mintChoice callback) [staticcall]
    │   │   │   └─ ← ()
    │   │   └─ ← ()
    │   └─ ← "EvmError: Revert"
    └─ ← "EvmError: Revert"

In this example, it will always revert because the target is not a contract and therefore does not have a `timeswapV2PoolMintChoiceCallback` function to call.

#### Recommended Mitigation

Include a router contract or something that end users of the frontend app will interact with which handles the callbacks. Or check if `msg.sender` is a contract before attempting the callback and use alternative logic if it's an EOA.

## L-02 - Uninitialized state variables

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-option/src/TimeswapV2OptionFactory.sol#L25

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L33 

The `getByIndex` state variables in both TimeswapV2OptionFactory and TimeswapV2PoolFactory are never initialized. 

Consequentially their respective `numberOfPairs` functions, which return the length of `getByIndex`, always return 0.

Example test in foundry, edited from TimeswapV2OptionFactory.t.sol

	function testOptionFactoryNumberOfPairs(address token0, address token1) public {
		console.log(factory.numberOfPairs());
		assertEq(0, factory.numberOfPairs());

		vm.assume(token1 > token0);
		vm.assume(token1 != address(0) && token0 != address(0));
		address opPair = factory.create(token0, token1);
		assertGt(0, factory.numberOfPairs());
	}

This test fails because `getByIndex` is never initialized or updated - its length will always be 0.

Also, the following unedited test from TimeswapV2PoolFactory.t.sol should not be asserting that `numberOfPairs` is equal to 0 after creating a pair. 

	function testNumberOfPairs() public {
        address token0 = address(1);
        address token1 = address(2);
        address opPair = optionFactory.create(token0, token1);
        address pool = poolFactory.create(opPair);
        assertEq(poolFactory.numberOfPairs(), 0);
    }

It should be asserting that the `numberOfPairs` result is greater than 0:

	assertGt(0, poolFactory.numberOfPairs());

And it should fail because `numberOfPairs` is never initialized.

#### Recommendation:

Add newly created pairs to the `getByIndex` array within the `create` functions of the respective factory contracts, or remove the variables and `numberOfPairs` functions altogether.

## L-03 - Missing-zero-checks 

The following contract constructors lack zero checks for address parameters:

1. OwnableTwoSteps

   https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/base/OwnableTwoSteps.sol#L18

2. TimeswapV2PoolFactory 

   https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L37

3. TimeswapV2LiquidityToken

   https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36 

4. TimeswapV2token

   https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L41

There is also a missing zero check in `get` within PoolFactory.sol:

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolFactory.sol#L13-L33

    /// @dev Reverts when pool factory address is zero.
    error ZeroFactoryAddress();


    /// @dev Checks if the pool factory address is zero.
    /// @param poolFactory The pool factory address which is needed to be checked.
    function checkNotZeroFactory(address poolFactory) internal pure {
        if (poolFactory == address(0)) revert ZeroFactoryAddress();
    }


    /// @dev Get the option pair address and pool pair address.
    /// @param optionFactory The option factory contract address.
    /// @param poolFactory The pool factory contract address.
    /// @param token0 The address of the smaller address ERC20 token contract.
    /// @param token1 The address of the larger address ERC20 token contract.
    /// @return optionPair The retreived option pair address. Zero address if not deployed.
    /// @return poolPair The retreived pool pair address. Zero address if not deployed.
    function get(address optionFactory, address poolFactory, address token0, address token1) internal view returns (address optionPair, address poolPair) {
        optionPair = optionFactory.get(token0, token1);


        poolPair = ITimeswapV2PoolFactory(poolFactory).get(optionPair);
    }

It appears that the unimplemented `checkNotZeroFactory` function and `ZeroFactoryAddress` error were intended to be used to check the `poolfactory` parameter in `get`, but they are not currently being used (in this or any other contract). 

Recommend implementing that function/error to perform the zero check that it was intended to perform.

## L-04 - Optimizer runs can cause errors

There is a specification of 200 optimizer runs in the hardhat.config.ts files for the v2-option, v2-pool, and v2-token workspaces. In general it's recommendeded not to use the optimizer for mainnet deployment since it's not yet thoroughly tested and can occasionally add bugs to otherwise working contracts. 

for reference: 
- https://docs.soliditylang.org/en/v0.8.17/bugs.html 
- https://docs.google.com/document/d/1PZBSCBWBwd6AqWCgXqLnw8FNQ4HRurP5usrXuKuU0a0/edit# 

It's recommended to avoid optimizations unless absolutely necessary. Weigh security concerns vs contract sizing and gas optimization needs.

## NC-01 - Error.zeroAddress use is inconsistent in TimeswapV2PoolFactory 

The error is used in line 60 if something IS the zero address and then on line 63 if something IS NOT the zero address (i.e. it already exists). 

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L60-L63 

    if (optionPair == address(0)) Error.zeroAddress();


    pair = pairs[optionPair];
    if (pair != address(0)) Error.zeroAddress();

The 2nd usage is inconsistent with its description in Error.sol:

	/// @dev Reverts when a given address is the zero address.
    error ZeroAddress();

The correct error message seems to be `PoolPairAlreadyExisted` from the PoolPairLibrary in PoolPair.sol

## NC-02 - checkDoesNotExist in PoolPairLibrary is not implemented 

The function `checkDoesNotExist(address optionPair, address poolPair)` is not implemented. 

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/PoolPair.sol#L22

Related to the above `error.zeroAddress` issue, line 63 of TimeswapV2PoolFactory.sol is performing the exact same check that occurs in `checkDoesNotExist`. Replacing line63 with `checkDoesNotExist(optionPair, pairs[optionPair])` would fix both issues as this function also has the correct error message internally.

    function checkDoesNotExist(address optionPair, address poolPair) internal pure {
        if (poolPair != address(0)) revert PoolPairAlreadyExisted(optionPair, poolPair);
    }

`checkDoesNotExist` could replace this:

    if (pair != address(0)) Error.zeroAddress();

at line 63: https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L63

## NC-03 unsafeDurationFromLastTimestampToNowOrMaturity is not implemented

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-pool/src/libraries/DurationCalculation.sol#L43

`unsafeDurationFromLastTimestampToNowOrMaturity` in DurationCalculation.sol is unimplemented. All the other functions from this library are used in structs/pool.sol but this one is never used.

Recommendation is to remove the unused function.

## NC-04 Console log still in TimeswapV2Token.sol 

https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2Token.sol#L109

This `console.log` is still in the code in TimeswapV2Token. Don't forget to remove it before deployment! 

It also has a typo in it (but obviously that won't matter once it's gone).

