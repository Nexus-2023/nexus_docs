# Nexus Bridge ETH

Nexus Bridge ETH contract is used by Rollup Bridge Contract to enable staking of ETH in their Bridge Contract. There are two ways to integrate Nexus Bridge Contract with Rollup and we'll talk about them later on. First, let's see what are common methods in all types of contracts:

1.  **Deposit Validator:** This is the main function that needs to be present in the Bridge Contract as it enables the creation of validators. This function can only be called from the Nexus Contract

    ```solidity
    function depositValidatorNexus(
            INexusInterface.Validator[] calldata _validators,
            uint256 stakingLimit
        )
    ```
2.  **Set Nexus Fee:** This function enables setting nexus fee percentage so that the fee is transferred from the rewards accumulated

    ```solidity
    function setNexusFee(uint256 _nexus_fee)
    ```
3.  **Update Slashing:** This function updates the slashing if it happens for a particular validator

    ```solidity
    function validatorsSlashed(uint256 amount)
    ```
4.  **Get Rewards Accumulated:** This function shows the amount of rewards that have accumulated and can be claimed.

    ```solidity
    function getRewards()
    ```
5.  **Exit Validator:** Whenever a validator exits and its balance is transferred back to the bridge contract, this function is called to update the exited validator count. This is to ensure that the Bridge Contract does not treat the exit balance as rewards distributed and make it available for claiming for any party involved.

    ```solidity
    function updateExitedValidators()
    ```

_<mark style="color:blue;">Claiming of rewards has different architectures and implements a few more functionalities apart from the above depending on the architecture selected by the Rollup for reward management.</mark>_&#x20;

### Integration:

For integration with the Nexus network a Rollup has two options:

1. **Deploy nexus package as a library:** If a Rollup Bridge Contract is more than 23KB, it needs to deploy the nexus package as a library. This way they can make _`delegatecall`_ to the library and perform the execution on their Bridge Contract
2. **Integrate nexus package with Bridge Contract Code:** If a Rollup Bridge Contract is less than 23KB, they can directly import the Nexus package in their Bridge Contract.
