# Nexus Contract

This contract is the heart and soul of Nexus Network. This contract is a UUPS upgradeable contract as new functionality needs to be added according to Rollup needs. The contract can be made non-upgradeable once the code is frozen. It is responsible for the following actions:

### Admin Management

1.  **Whitelist Rollup:** Before rollup is registered with Nexus Network, an address is whitelisted from the rollup side that performs all the rollup actions. This is to ensure that no other person can register their bridge contract before them. This action is performed by Nexus Admin.

    <pre class="language-solidity"><code class="lang-solidity"><strong>function whitelistRollup(
    </strong>        string calldata name,
            address rollupAddress
        )
    </code></pre>
2.  **Set Node Operator:** This function sets the Node Operator Contract address.

    ```solidity
    function setNodeOperatorContract(address _nodeOperator)
    ```


3.  **Change Execution Fee Recipient Address:** Proposer rewards are sent to a different address for validators**.** This function sets the Node Operator Contract address.

    <pre class="language-solidity"><code class="lang-solidity"><strong>function changeExecutionFeeAddress(address _execution_fee_address)
    </strong></code></pre>

### Rollup Management

1.  **Register Rollup:** This function is used by Rollup to register their rollup with Nexus Network

    ```solidity
    registerRollup(address bridgeContract,uint64 operatorCluster,uint256 nexusFee,uint16 stakingLimit)
    ```

    Parameters:

    1. **bridgeContract**: This is the bridge contract address that has the Nexus Network package integrated.&#x20;
    2. **operatorCluster**: Set of NodeOperator as selected by the Rollup.
    3. **nexusFee**: Fee set by rollup as part of staking rewards. It can be between 0-1000 translate to 0-10% .&#x20;

    _<mark style="color:blue;">We recommend that it should be 10% as it helps us to give a fair pay to Node Operators and manage our operational cost.</mark>_
2. **Change Rollup Parameters**
   1.  **Changing Staking Limit**: Rollup can use this function to change their staking limit.

       ```solidity
       changeStakingLimit(uint16 newStakingLimit)
       ```
   2.  **Change Nexus Fee**: Rollup can use this function to change Nexus Fee &#x20;

       ```solidity
       function changeNexusFee(uint256 _new_fee)
       ```
   3.  **Change Cluster**: If rollup wants to change the NodeOperator set, they can use this function

       ```solidity
       function changeCluster(uint64 operatorCluster)
       ```

### Validator Management

Validators are the backbone of any staking infrastructure and likewise are one of the most important component of SSV:

1.  **Depositing Validator:** This function is used to deposit a validator to respective rollup bridge

    ```solidity
    struct Validator {
            bytes pubKey;
            bytes withdrawalAddress;
            bytes signature;
            bytes32 depositRoot;
        }
    function depositValidatorRollup(address _rollupAdmin,Validator[] calldata _validators)
    ```

    Parameters:

    1. **\_rollupAdmin**: Address for rollup admin that handles all the parameters for rollup on Nexus.&#x20;
    2. **\_validators**: List of validators that need to be deposited to bridge Contract for activation
2.  **Slashing Validator:** This function updates the slashing for validator of a particular rollup

    ```solidity
    function validatorSlashed(address rollupAdmin, uint256 amountSlashed)
    ```
3.  **Validator Exit:** This function updates the validator exit status in the Nexus Contract

    ```solidity
    function validatorExit(address rollupAdmin,bytes[] calldata pubkeys)
    ```
4.  **Validator exit Balance transfer:** This function i used when the validator balance is transferred to the Bridge Contract. It performs two major function, one is to tell the Bridge contract about the balance transfer and the other to inform the Node Operators that they can stop performing the validation for the validator.

    ```solidity
    function validatorExitBalanceTransferred(address rollupAdmin,bytes calldata pubkey, uint64[] memory operatorIds, ISSVNetworkCore.Cluster memory cluster)
    ```



### SSV Integration

1.  **Deposit SSV Shares:** This function submits the keyshares to the SSV contract so that the Node Operators can start the validator operations

    ```solidity
    function depositValidatorShares(
            address _rollupAdmin,
            ValidatorShares calldata _validatorShare
        )
    ```
2.  **Recharge SSV cluster:** This function pays the SSV fees by sending money to SSV contract. From there the node operators can claim their fee

    ```solidity
    function rechargeCluster(uint64 clusterId, uint256 amount,ISSVNetworkCore.Cluster memory cluster)
    ```
