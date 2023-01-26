## Title: 
No `to`  address check for the `transferFeesFrom` function.

## Description:
In the code, the function transferFeesFrom allows for transferring fees from one address (from) to another address (to). However, it does not check if the `to` address is the same as the `msg.sender` before transferring the fees.
`if (!isApprovedForAll(from, msg.sender)) revert NotApprovedToTransferFees();`

## Impact:
This means that any address can call this function and transfer the fees to any address they specify, regardless of whether they are the intended recipient or not. This could allow an attacker to transfer fees to an address they control, potentially stealing the fees from the intended recipient. The lack of this check allows any address to call the function and transfer the fees to any address they want, which can lead to the unauthorized transfer of funds.
It is important to check that the `to` address is the intended recipient of the fees being transferred before allowing the transfer to occur because it helps to ensure that the funds are going to the correct recipient. Without this check, any address could call the function and transfer the fees to any address they specify. This could allow an attacker to transfer fees to an address they control, potentially stealing the fees from the intended recipient.

Additionally, it is a best practice to ensure that the intended recipient of funds is confirmed before the transfer occurs, to prevent unauthorized or accidental transfers to the wrong address. It also ensures the integrity of the funds and the security of the smart contract, as it prevents unauthorized access to the funds.

By making sure that the `to`address is the intended recipient, the smart contract can be more robust and secure, and it can provide a better user experience.

## PoC:

[TimeswapV2LiquidityToken.sol#L75-L96](https://github.com/code-423n4/2023-01-timeswap/blob/ef4c84fb8535aad8abd6b67cc45d994337ec4514/packages/v2-token/src/TimeswapV2LiquidityToken.sol#L75-L96)

```
  function transferFeesFrom(address from, address to, TimeswapV2LiquidityTokenPosition calldata position, uint256 long0Fees, uint256 long1Fees, uint256 shortFees) external override {
        if (from == address(0)) Error.zeroAddress();
        if (to == address(0)) Error.zeroAddress();

        if (!isApprovedForAll(from, msg.sender)) revert NotApprovedToTransferFees();

        uint256 id = _timeswapV2LiquidityTokenPositionIds[position.toKey()];

        if (long0Fees != 0 || long1Fees != 0 || shortFees != 0) _addTokenEnumeration(from, to, id, 0);

        // add/mint the fees for the new user
        _feesPositions[id][to].mint(long0Fees, long1Fees, shortFees);

        _updateFeesPositions(from, to, id);

        // remove/burn the fees
        _feesPositions[id][from].burn(long0Fees, long1Fees, shortFees);

        if (long0Fees != 0 || long1Fees != 0 || shortFees != 0) _removeTokenEnumeration(from, to, id, 0);

        emit TransferFees(from, to, position, long0Fees, long1Fees, shortFees);
    }
```
## Tools: 
Manual code Review