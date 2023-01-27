### 1. USE `block.timestamp` DIRECTLY TO GET THE CURRENT BLOCK TIMESTAMP WITHOUT CALLING THE INTERNAL FUNCTION `blockTimestamp(0)`

        if (blockTimestamp(0) > maturity) Error.alreadyMatured(maturity, blockTimestamp(0));
		
This code can be re-written as follows:

        if (blockTimestamp(0) > maturity) Error.alreadyMatured(maturity, block.timestamp);

Calling the internal function and doing arithmetic operation will consume additional gas for the specific usecase of `blockTimestamp(0)`.
since `blockTimestamp(0)` represents the `block.timestamp` it recommended to use the latter to save gas rather than unnecessarily calling the `internal` function

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L155

There are 2 more instances of this issue:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L159
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L169


### 2. 	CALLDATA VARAIBLES CAN BE USED TO EMIT AN EVENT INSTEAD OF USING STATE VARIABLE.

        pendingOwner = chosenPendingOwner;

        emit SetOwner(pendingOwner);
		
		
In the above code `pendingOwner` is a state variable and chosenPendingOwner is calldata variable passed into the function as a parameter.		
Above code can be rewritten as follows to save on one `SLOAD` and save gas by providing calldata variable to the event.

        pendingOwner = chosenPendingOwner;

        emit SetOwner(chosenPendingOwner);
		
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L29-L31

### 3. PRIVATE FUNCTIONS WHICH ARE NOT CALLED INSIDE THE CONTRACT CAN BE REMOVED.

The private function `feeOverflow()` in the `FeeCalculaton.sol` is not used inside the contract.
Hence this can be removed from the contract. This will reduce the contract deployment gas cost.

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/libraries/FeeCalculation.sol#L15-L17