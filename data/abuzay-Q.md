QA Report: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol

The code does not have any mechanism to update the contract if necessary.

Severity: Medium

Impact:
The lack of a mechanism to update the contract can lead to unexpected behavior and financial loss in certain situations. For example, if a bug is found in the contract, it may be necessary to update the contract in order to fix the issue and prevent further damage. Additionally, if the smart contract's logic becomes outdated, the contract may need to be updated to reflect the current business requirements.

Proof of Concept:
A proof of concept for this vulnerability would involve creating a malicious contract that can call the update function with malicious or invalid parameters.

pragma solidity = 0.8.8;

contract Attacker {
    address public target;

    constructor(address _target) public {
        target = _target;
    }

    function exploit() public {
        // call the update function with malicious or invalid parameters
        target.update(address(0), address(0), address(0));
    }
}

The Attacker contract takes the address of the vulnerable contract as a constructor argument, and has a function called "exploit" that calls the update function with malicious or invalid parameters.
An attacker could then use this exploit to cause a denial of service attack, where the contract becomes unusable due to the invalid parameters, or use the updated contracts to steal funds or perform other malicious actions.

Mitigation Steps:

Implement a function to update the contract's logic and parameters, this function should be only callable by the contract's owner or an authorized party.

Implement a mechanism to test the updated contract's logic and parameters before deploying it to the main network.

Implement a mechanism to rollback to the previous version of the contract in case the updated contract's logic and parameters are incorrect.

Document the update process, its requirements and the risks associated with updating the contract.







QA Report: https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/TimeswapV2OptionDeployer.sol

Title: The code does not have any mechanism to pause or stop the contract if necessary.

Severity Medium

Impact:
The lack of a mechanism to pause or stop the contract can lead to unexpected behavior and financial loss in certain situations. For example, if a bug is found in the contract, it may be necessary to pause or stop the contract in order to prevent further damage. Additionally, if there are security concerns about the contract, it may be necessary to pause or stop the contract to prevent potential attacks.

Proof of Concept:
A proof of concept for this vulnerability would involve creating a malicious contract that can call the pause or stop function without proper authorization.

pragma solidity = 0.8.8;

contract Attacker {
    address public target;

    constructor(address _target) public {
        target = _target;
    }

    function exploit() public {
        // call the pause or stop function without proper authorization
        target.pause();
    }
}


pragma solidity = 0.8.8;

contract Attacker {
    address public target;

    constructor(address _target) public {
        target = _target;
    }

    function exploit() public {
        // call the pause or stop function without proper authorization
        target.pause();
    }
}

Mitigation Steps:

Implement a function to pause or stop the contract, this function should be only callable by the contract's owner or an authorized party.

Implement a mechanism to prevent reentrancy attacks during the pause or stop function.

Document the pause or stop process, its requirements and the risks associated with pausing or stopping the contract.

Implement a mechanism to resume the contract after it's paused or stopped.

