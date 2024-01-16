# Different Nexus Bridge Architecture

There are two possible bridge architectures based on the reward management selected by the rollup -&#x20;

## **Rewards managed by rollup governance**

This allows the rollup governance to claim the rewards and use them for the growth of rollup ecosystem. Some of the possible use cases are:

1. Fund the infrastructure cost of the rollup
2. Fund the developer activity
3. Incentivise ecosystem growth&#x20;
4. Incentivise the governance token

_<mark style="color:blue;">There can be many possibilities depending on the choice of the rollup, and Nexus Network allows full autonomy to the rollup to manage the rewards.</mark>_

The rollup must import the following function to allow the DAO to collect rewards -

**RedeemRewards:** A DAO address is added to the bridge contract that can collect the rewards and send them to any address they want by using this function

```solidity
function redeemRewards(address reward_account) onlyDAO
```

## **Distribute to rollup users**

The rollup can also distribute the rewards back to the rollup users. The rewards can be distributed to the users in two ways based on the design choice taken by the rollup -&#x20;

### Compound Token (c-token)

The rollup can implement a c-token model where the value of their asset holding on the rollup (ETH, stablecoins) keeps increasing based on the rewards accrued. The rollup bridge maintains the ratio (c-value) of all the rewards earned by the rollup and the total ETH bridged to the rollup. The c-value is also used when minting new ETH on the rollup. The following functions are used to implement the same:

**Update C Value**: This function changes the `cValue` that is used to calculate the amount of ETH that has to be minted on the Rollup.

```solidity
uint256 public cValue; // Variable that stores the reward to eth transferred ratio
function updateCValue()
```

bridgeBalance = amountDeposited - amountWithdrawn

$$cValue= bridgeBalance/(bridgeBalance+rewards)$$

The rollup will store two additional variables on the bridge, _amountDeposited_ and _amountWithdrawn_ that will track the deposits and withdrawals from the rollup bridge

### Rebase Token

The rebase token model updates the ETH balance of users on the rollup after regular intervals based on the rewards earned. The rollup sequencer mints ETH equal to rewards earned to all the addresses on the rollup holding ETH. This ensures that the total ETH on Ethereum L1 is equal to the ETH on the rollup. The following function is used to implement the same:

**Rebase**: This function calculates the amount that needs to be distributed to the users on the rollup and emits an event `RebaseAmount` so that the sequencer can distribute it

```solidity
uint256 public amountDistributed; // amount already distributed
event RebaseAmount(uint256 amount); // event used by sequencer to distribute the reards 
function rebase()
```

_<mark style="color:blue;">The rollup sequencer needs to be updated for this as the ETH balance of each user is calculated at the time of rebasing and changes are made in the state of the rollup accordingly</mark>_
