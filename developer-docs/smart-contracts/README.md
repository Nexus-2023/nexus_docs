# 📔 Smart Contracts

Smart Contracts are a source of truth that stores logic for various rollups to integrate and maintain the parameters set by rollup. For the nexus network to work, the following set of smart contracts is needed:

1. [**Nexus Contract**](https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/Nexus.sol)**:** This contract is responsible for storing the parameters of rollup necessary to start staking. It also keeps track of all the validators that are registered through Nexus Network.&#x20;
2. [**Node Operator Contract**: ](https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/NodeOperator.sol)This contract handles all the node operator operations and clusters needed for running validators and keeping score
3. [**Nexus Bridge ETH**](https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/nexus\_bridge/NexusBaseBridge.sol): This contract needs to be implemented with the bridge contract present on any Rollup. It enables the staking of ETH from the Bridge Contract.
4. [**Nexus Bridge DAI**](https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/nexus\_bridge/NexusDAIBridge.sol): This contract is responsible for enabling DAI deposit in [DSR](https://blog.makerdao.com/dai-savings-rate/) savings contract from the Bridge Contract
5. [**Validator Execution Rewards**](https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/ValidatorExecutionRewards.sol): This contract gets all the proposer rewards for the validators and enables Rollups to claim those rewards.
