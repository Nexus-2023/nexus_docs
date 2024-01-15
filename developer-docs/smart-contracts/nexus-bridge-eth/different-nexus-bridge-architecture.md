# Different Nexus Bridge Architecture

Bridge architecture depends on how Rollup wants to manage the rewards earned from staking. Two possible scenarios are:

1.  **DAO governance**: A Rollup can claim the rewards and use them for any of the following use cases:

    1. Fund the infrastructure cost
    2. Fund the developer activity
    3. Fund ecosystem growth
    4. Incentivise the governance token

    _<mark style="color:blue;">Apart from this, there can be other possibilities and it depends on the rollup to manage the rewards.</mark>_

    To implement the same one need to implement following function

    1.  **RedeemRewards:** A DAO address is added to the bridge contract and they can send the rewards collected to any address they want by using this function.

        ```solidity
        function redeemRewards(address reward_account) onlyDAO
        ```
2. **User distribution:** The Rollup can also distribute the rewards back to the user as well. This can be done by selecting a token-minting approach on the Rollup for ETH bridged:&#x20;

### Compound Token(c-token)

One can implement a c-token model where one keeps track of all the rewards earned by the Rollup and the total ETH bridged to the Rollup. The ratio of the two is used when minting new ETH on Rollup. The following functions are used to implement the same:

1.  **Update C Value**: This function changes the `cValue` that is used to calculate the amount of ETH that has to be minted on the Rollup.

    ```solidity
    uint256 public cValue; // Variable that stores the reward to eth transferred ratio
    function updateCValue()
    ```

    bridgeBalance = amountDeposited - amountWithdrawn

    $$cValue= bridgeBalance/(bridgeBalance+rewards)$$

### Rebase Token

Rebase token on the other hand changes the balance of users on the Rollup so that the total ETH on Ethereum L1 on Bridge Contract is equal to the total ETH on the Rollup. To implement this one needs to distribute the rewards earned and not distributed to the Users on Rollup that hold the Rollup ETH. The following function is used to implement the same:

1.  **Rebase**: This function calculates the amount that needs to be distributed to the users on the Rollup and emits an event `RebaseAmount` so that the sequencer can distribute it.

    ```solidity
    uint256 public amountDistributed; // amount already distributed
    event RebaseAmount(uint256 amount); // event used by sequencer to distribute the reards 
    function rebase()
    ```

_<mark style="color:blue;">For this to work, the Rollup will need to make changes in their sequencer as they'd need to know how much ETH each user has on rollup at the time of rebasing and make changes in the state of Rollup accordingly</mark>_
