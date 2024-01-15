# Nexus Bridge DAI

Nexus Bridge DAI is used to integrate any Rollup Bridge Contract with the DSR saving contract from MakerDAO and earn extra yield whenever a user deposits DAI in the contract. It has the following functions to perform the action:

1.  **Deposit DAI**: This function uses the MakerDAO DSR contract and deposits the DAI sent to bridge in it. This is performed whenever the user deposits funds to the Bridge Contract

    ```solidity
    function depositDai(uint256 amountSave)
    ```
2.  **Remove DAI**: This function removes DAI from the DSR contract and gives it back to the user whenever they claim the DAI from the bridge

    ```solidity
    function removeDai(address user, uint256 amountRedeem)
    ```
3.  **Claim Rewards DAI**: This function is used by the Rollup governance to claim the DSR rewards and sends it to the address that the Rollup wants to

    ```solidity
    function claimRewardsDAI(address address_to_send)
    ```

_<mark style="color:blue;">As different architectures are present for the claiming of ETH rewards, the same fundamentals can be applied to the DAI rewards as well.</mark>_
