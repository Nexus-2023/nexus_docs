# Nexus Bridge ETH

Nexus Bridge ETH contract is used by rollup bridge to enable staking of ETH locked in their bridge contract. Nexus Network offers two possible designs for rollups when they integrate Nexus bridge contracts which are detailed [here](different-nexus-bridge-architecture.md). This page elaborates on the common methods in both of the possible designs:

1.  **Deposit Validator:** This is the main function that needs to be present in the rollup bridge contract as it enables the creation of validators. This function can only be called from the Nexus Contract

    ```solidity
    function depositValidatorNexus(
            INexusInterface.Validator[] calldata _validators,
            uint256 stakingLimit
        )
    ```


2.  **Set Nexus Fee:** This function enables setting a fee percentage for Nexus Network that is transferred from the rewards accumulated

    ```solidity
    function setNexusFee(uint256 _nexus_fee)
    ```


3.  **Update Slashing:** This function updates the slashing details if it happens for a particular validator

    ```solidity
    function validatorsSlashed(uint256 amount)
    ```


4.  **Get Rewards Accumulated:** This function shows the amount of rewards that have accumulated and can be claimed.

    ```solidity
    function getRewards()
    ```
5.  **Exit Validator:** Whenever a validator exits and its balance is transferred back to the bridge contract, this function is called to update the exited validator count. This ensures that the bridge contract can differentiate between ETH received through validator exits and staking returns.

    ```solidity
    function updateExitedValidators()
    ```

_<mark style="color:blue;">Claiming of staking rewards has two possible architectures and implements a few more functionalities apart from the above depending on the architecture selected by the Rollup for reward management.</mark>_&#x20;

### Integration:

For integration with Nexus Network, a rollup has two options:

1. **Deploy nexus-package as a library:** If a rollup bridge contract is more than 23 kb in size, it needs to deploy the nexus-package as a library. This way they can make _`delegatecall`_ to the library and perform the execution on their bridge contract
2. **Integrate the nexus-package with bridge contract code:** If a rollup bridge contract is less than 23 kb in size, it can directly import the nexus-package in its bridge contract
