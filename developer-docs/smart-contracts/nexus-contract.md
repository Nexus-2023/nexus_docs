# Nexus Contract

This contract is the heart and soul of Nexus Network. This contract is a **UUPS upgradeable** contract that allows the addition of new functionalities according to the needs of the rollups. The contract can be made non-upgradeable once the code is frozen. It is responsible for the following actions:

### Admin Management

1.  **Whitelist Rollup:** A roll looking to register with Nexus Network, must get an address whitelisted by Nexus Admin. The address acts as the rollup admin and performs all the rollup actions. This whitelisting process removes any potential spamming in the name of a particular rollup

    <pre class="language-solidity"><code class="lang-solidity"><strong>function whitelistRollup(
    </strong>        string calldata name,
            address rollupAddress
        )
    </code></pre>


2.  **Set Node Operator:** This function sets the Node Operator Contract address

    ```solidity
    function setNodeOperatorContract(address _nodeOperator)
    ```


3.  **Set Off Chain oracle:** This function sets the off-chain oracle bot:

    ```solidity
    function setOffChainBot(address _botAddress) 
    ```
4.  **Change Execution Fee Recipient Address:** Block proposer rewards earned by validators are sent to a different wallet address to prevent any potential confusion for the sequencer**.** The changeExecutionFeeAddress() function sets the execution fee recipient address

    <pre class="language-solidity"><code class="lang-solidity"><strong>function changeExecutionFeeAddress(address _execution_fee_address)
    </strong></code></pre>

### Rollup Management

1.  **Register Rollup:** This function is used by rollups to register their rollup with Nexus Network

    ```solidity
    registerRollup(address bridgeContract,uint64 operatorCluster,uint256 nexusFee,uint16 stakingLimit)
    ```

    \
    Parameters:

    1. **bridgeContract**: This is the modified bridge contract address that has the Nexus Network package integrated&#x20;
    2. **operatorCluster**: Set of node operators as selected by the rollup. If the rollups do not have a preference, Nexus Network will set the operatorCluster to the most performant clusters
    3. **nexusFee**: Sets the percentage of staking returns that go to Nexus Network as fees. It is set by the rollup in consultation with the Nexus Network team.
    4. **stakingLimit:** Set the staking limit for the bridge contract. This determines percentage of ETH that needs to be staked.\

2. **Change Rollup Parameters**
   1.  **Changing Staking Limit**: Rollups can use this function to change their staking limit.

       ```solidity
       function changeStakingLimit(uint16 newStakingLimit)
       ```
   2.  **Change Nexus Fee**: Rollups can use this function to set a new fee for Nexus Network &#x20;

       ```solidity
       function changeNexusFee(uint256 _new_fee)
       ```
   3.  **Change Cluster**: If rollup wants to change the NodeOperator set, they can use this function

       ```solidity
       function changeCluster(uint64 operatorCluster)
       ```

### Validator Management

Validators are the backbone of any staking infrastructure and likewise are one of the most important components of SSV:

1.  **Deposit Validator:** This function is used to deposit a validator to the respective rollup bridge

    ```solidity
    struct Validator {
            bytes pubKey;
            bytes withdrawalAddress;
            bytes signature;
            bytes32 depositRoot;
        }
    function depositValidatorRollup(address _rollupAdmin,Validator[] calldata _validators)
    ```

    \
    Parameters:

    1. **\_rollupAdmin**: Address for rollup admin that can modify the rollup parameters on Nexus&#x20;
    2. **\_validators**: List of validators that need to be deposited to the bridge contract for activation\

2.  **Slashing Validator:** This function picks up the slashing details from oracles and updates it for a particular rollup

    ```solidity
    function validatorSlashed(address rollupAdmin, uint256 amountSlashed)
    ```


3.  **Validator Exit:** This function updates the validator exit status in the Nexus Contract

    ```solidity
    function validatorExit(address rollupAdmin,bytes[] calldata pubkeys)
    ```


4.  **Validator exit balance transfer:** Nexus Network oracles communicate with this function to update whenever an exited validator balance is transferred to the rollup bridge. Whenever the function receives an update, it communicates the rollup bridge contract about the balance transfer and informs the Node Operators to stop performing the validation for the validator

    ```solidity
    function validatorExitBalanceTransferred(address rollupAdmin,bytes calldata pubkey, uint64[] memory operatorIds, ISSVNetworkCore.Cluster memory cluster)
    ```

### SSV Integration

1.  **Deposit SSV Shares:** This function submits the keyshares generated after validator creation to the SSV contract so that the SSV node operators can start the validator operations

    ```solidity
    function depositValidatorShares(
            address _rollupAdmin,
            ValidatorShares calldata _validatorShare
        )
    ```


2.  **Recharge SSV cluster:** This function pays the SSV fees by sending money to the SSV contract. This includes fees for node operators as well as SSV specific fees

    ```solidity
    function rechargeCluster(uint64 clusterId, uint256 amount,ISSVNetworkCore.Cluster memory cluster)
    ```
