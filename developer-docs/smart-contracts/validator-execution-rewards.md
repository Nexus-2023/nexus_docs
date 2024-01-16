# Validator Execution Rewards

Ethereum validators are rewarded in two ways -

1. **Consensus rewards**: All validators get rewards sent for attesting Ethereum blocks that get sent directly to their withdrawal address
2. **Execution/Block Proposer rewards**: Validators earn this reward as tips paid whenever they get the opportunity to propose a block. These rewards are sent to the address that is present with the validator client and the address can be different from the consensus award address and can be changed whenever a person wants.

For Nexus Network, the proposer rewards are directed to the Validator Execution Reward Contract. The contract stores all the proposer rewards and addresses for each rollup associated with the validator. The rollup can claim these awards whenever their validator proposes a block. The following functions are used for the implementation:

1.  **Change Reward Bot Address**: Nexus Network uses this function to set a bot address that fetches information about the incoming rewards and updates for allocation of rewards to particular rollups

    ```solidity
    function changeRewardBotAddress(address _bot_address)
    ```
2.  **Update Reward Rollup**: The bot registered in the previous step updated the details on rewards using this function. It contains all the rollup proposer rewards that have been earned by their validators

    ```solidity
    function updateRewardsRollup(RollupExecutionReward[] calldata rewards)
    ```
3.  **Claim Reward**: Rollups can use this function to claim their pending proposer rewards

    ```solidity
    function claimRewards()
    ```
