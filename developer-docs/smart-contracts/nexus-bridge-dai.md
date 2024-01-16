# Nexus Bridge DAI

Nexus Bridge DAI allows a rollup to deposit the DAI locked in the rollup bridge to the DSR saving contract from MakerDAO and earn yields. These are the functions used to enable the feature:

1.  **Deposit DAI**: This function deposits the DAI on rollup bridge to the Maker DAO DSR contract. It is an automated action that takes place whenever a user deposits DAI to the rollup

    ```solidity
    function depositDai(uint256 amountSave)
    ```


2.  **Remove DAI**: This function removes DAI from the DSR contract and gives it back to the user whenever they claim the DAI from the bridge

    ```solidity
    function removeDai(address user, uint256 amountRedeem)
    ```


3.  **Claim Rewards DAI**: This function is used by the rollup governance to claim and utilize the DSR rewards

    ```solidity
    function claimRewardsDAI(address address_to_send)
    ```

_<mark style="color:blue;">The design architectures for DAI can be customized in a similar manner to ETH, which is detailed</mark>_ [_<mark style="color:blue;">here</mark>_](nexus-bridge-eth/different-nexus-bridge-architecture.md)
