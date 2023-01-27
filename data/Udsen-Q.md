### 1. USING VULNERABLE DEPENDENCY OF OPENZEPPELIN

According to the package.json file, the project is using 4.7.0, to be exact ^4.7.2 which is not the latest updated version.

    "dependencies": {
      "@openzeppelin/contracts": "^4.7.2"

VULNERABILITY	VULNERABLE VERSION
H    	Improper Verification of Cryptographic Signature	<4.7.3

It is recommended to use the latest non vulnerable version 4.8.0

### 2. USE MORE RECENT VERSION OF SOLIDITY

Old version of solidity is used (^0.8.8), but newer version 0.8.17 can be used.

For security, it is recommended to use the latest solidity version.
security fix list for different solidity versions can be found in the following link.

https://github.com/ethereum/solidity/blob/develop/Changelog.md

Above issue is found in all contracts.

### 3. NATSPEC IS MISSING

NatSpec is missing for the following contracts, functions, constructor and modifier

`TimeswapV2LiquidityToken.sol` is a main contract of the project. But NatSpec is missing in the code. 
	
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L1-L281

It is recommended to use the NatSpec for commenting for better readability and understanding of the code base.
	
There are 4 more instances of this issue:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L1-L57
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol#L1-L519
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2PoolFactory.sol#L1-L71
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol#L1-L112

### 4. SHOULD PERFORM THE `chosenOptionFactory != address(0)` FOR THE NULL ADDRESS CHECK WHEN PASSING IN ADDRESS PARAMETERS INTO THE CONSTRUCTOR

    constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {
        optionFactory = chosenOptionFactory;
        poolFactory = chosenPoolFactory;
    }

It is a best practice to check for the `address(0)` when assigning address values to the state variables in solidity.

Above code can be re-written as follows:

    constructor(address chosenOptionFactory, address chosenPoolFactory) ERC1155("Timeswap V2 uint160 address") {
        if (chosenOptionFactory == address(0)) Error.zeroAddress();
        if (chosenPoolFactory == address(0)) Error.zeroAddress();		
        optionFactory = chosenOptionFactory;
        poolFactory = chosenPoolFactory;
    } 

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L36-L39

There are 2 more instances of this issue:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol#L42
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L19

### 5. LIBRARY INTERNAL FUNCTIONS ARE CALLED FROM THE PUBLIC API OF THE LIBRARY.

The best practice is to link the library to the contract by using the `using` keyword. 
But in the `TimeswapV2LiquidityToken.sol` file under the `mint` function the `check` internal function of library `ParaLibrary` is called as follows:

    function mint(TimeswapV2LiquidityTokenMintParam calldata param) external returns (bytes memory data) {
        ParamLibrary.check(param);
		
But `ParamLibrary` is not linked to the contract using the `using` keyword. Hence the `check` internal function is called using the public API of `ParamLibrary`.
This is allowed in the compiler for `solidity version 0.8.8`. But in the future solidity versions this could lead to compile time error.

Hence it is adviced to link the `ParamLibrary` to the `TimeswapV2LiquidityToken.sol` using the `using` keyword as follows:

    using ParamLibrary for TimeswapV2LiquidityTokenMintParam;  

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L100

There are 7 more instances of this issue:

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L122
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L151	
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L157
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L190
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L244
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L270
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionFactory.sol#L31

### 6. RECOMMENDED TO USE OPENZEPPELIN LIBRARIES FOR MATH OPERATIONS AND OWNERSHIP TRANSFER IN TWO STEPS.

`openzeppelin` libraries such as `Math.sol` and `safeMath.sol` are tested and proven codes used by many applications.
Hence it is recommended to use `openzeppelin` libraries for math operations by importing them and then customizing the contracts according to the application specific requirements.

In the timeswap project locally coded math libraries (`FullMath.sol`,`Math.sol`) have been used. This deprives users of the benefits of highly secure and battle tested `openzeppelin` math libraries which are prone to less errors.

Further to the above the two step ownership transfer logic is implemented via in-house coded `OwnableTwoSteps.sol`. But instead it is recommended to use the tried and tested openzeppelin `Ownable2Step.sol` for two step ownership transfer.

### 7. DIVISOR NOT CHECKED FOR ZERO VALUE.

    function div(uint256 dividend, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
        quotient = dividend / divisor;
		
In the `Math.sol` library, the `div` function is implementing the division of two numbers.
But it is not checking the divisor for the zero value. This could trigger unwanted implications since Math.sol library is used by other libraries (FullMath.sol) and smart contracts.

The above code should implement the zero value check for the divisor as follows:

    function div(uint256 dividend, uint256 divisor, bool roundUp) internal pure returns (uint256 quotient) {
		require(divisor != uint256(0), "Division by Zero");
        quotient = dividend / divisor; 	
		
https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-library/src/Math.sol#L48-L49

### 8. IT IS RECOMMENDED TO USE BOTH OLD AND NEW VALUES WHEN EMITTING EVENTS FOR VARAIBLE STATE CHANGES.

        msg.sender.checkIfPendingOwner(pendingOwner);
        owner = msg.sender;
        delete pendingOwner;
        emit AcceptOwner(msg.sender); 
		
Above code only logs the new owner address in the log. But as a best practice it is recommended to log the old owner details as well for future use.
So the above code can be refactored as follows:

        msg.sender.checkIfPendingOwner(pendingOwner);
		address oldOwner = owner;
        owner = msg.sender;
        delete pendingOwner;
        emit AcceptOwner(oldOwner, msg.sender);

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/base/OwnableTwoSteps.sol#L35-L42

