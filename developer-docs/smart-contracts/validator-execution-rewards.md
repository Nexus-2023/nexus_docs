# Validator Execution Rewards

Validators on Ethereum earn 2 types of awards:

1. **The consensus award**: This is the award directly sent to the withdrawal address of the validators for attesting the blocks.&#x20;
2. **Execution/Proposer awards**: These awards are sent to the address that is present with the validator client and the address can be different from the consensus award address and can be changed whenever a person wants.

For the nexus network, the proposer awards are sent to the Validator Execution Reward Contract. It stores all proposer rewards and addresses for each Rollup associated with the validator. The rollup then can claim these awards whenever their validator proposes a block. To implement the same following functions are required:

1.  **Change Reward Bot Address**: The nexus network uses this function to set a bot address that updates the proposer rewards of rollup.

    ```solidity
    function changeRewardBotAddress(address _bot_address)
    ```
2.  **Update Reward Rollup**: The bot sends the transaction using this function. It contains all the rollup proposer rewards their validators have earned.

    ```solidity
    function updateRewardsRollup(RollupExecutionReward[] calldata rewards)
    ```
3.  **Claim Reward**: Rollups can use this function to claim their pending porposer rewards

    ```solidity
    function claimRewards()
    ```
