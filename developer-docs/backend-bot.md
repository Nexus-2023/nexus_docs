# 🤖 Backend Bot

The backend bot continuously monitors the rollup bridge contracts including the current stake, staking limit set by the rollup, validator withdrawals, etc. It also initiates the validator key creation process and the validator exits

### Monitoring of staking limit

Every rollup integrated with Nexus Network sets a staking limit, i.e. the proportion of ETH locked in the bridge to be staked by Nexus Network. The backend bot monitors the rollup bridge contracts and creates keys and exits depending on the staking limit set by the rollup:

1. **When the current staking limit is less than the staking limit set by the rollup:** In this scenario, the following steps are executed -
   1. The backend bot calculates the number of validators to be created to reach the staking limit
   2. The validators are created and deposit data for validators is sent to the Nexus contract. Nexus contract sends it to the respective rollup bridge for activation
   3. The keyshares data is sent to the Nexus Contract which sends it to the SSV contract for registration, after which the Node operator can start performing validator duties\

2. **When the current staking limit is more than the staking limit set by the rollup**: In this scenario, the following steps are executed;
   1. The backend bot calculates the number of validators to be exited to reach the staking limit
   2. The exit message is created for the validators and is broadcast to the consensus chain
   3. The bot informs the Nexus contract of all the exiting validators. This ensures that the Nexus contracts can keep track of validators that are pending exit

_<mark style="color:blue;">The bot is deployed on AWS EC2 instance with limited access to the world. It can only be accessed by a single ssh key and that is stored offline and is only used for deployment.</mark>_

### Validator Key Management

Validator Key Management is crucial to maintain the security of the system. It can be done in two ways:

#### Centralized Key Management(<mark style="color:red;">Depreciated</mark>)

Centralized keys make it necessary to store the keys and mnemonic (if stored). Nexus Network manages the keys for a centralized key management system in the following ways:

1.  Whenever a new rollup is registered, a new mnemonic is created and stored in an AWS vault

    _<mark style="color:blue;">The mnemonic is always encrypted using a salt and the salt is only passed when running the script so that it is stored in memory and nowhere else. A backup for the salt is stored offline</mark>_
2. Whenever new keys need to be created, the mnemonic is fetched from the AWS vault and used in the creation of keys. The nonce is stored in a statefile and fetched from there
3. After creating the keys, [SSV CLI](https://github.com/bloxapp/ssv-keys) is used to create validator keyshares
4. The keyshares and deposit data for validators are deposited to the Nexus Contract according to the logic detailed above
5. When the deposit is complete, the keys are destroyed as mnemonic and nonce are the only things needed to recreate keys or exits

_<mark style="color:blue;">The AWS vault is only accessible by the AWS instance on which the backend bot is running by IAM roles</mark>_&#x20;

#### Decentralized Key Management (DKG)

To decentralize the key management system, the keys are generated in a decentralized manner. For this, we use the [SSV DKG](https://docs.ssv.network/operator-user-guides/operator-node/enabling-dkg).&#x20;

Process for DKG:

1. Node Operators run the DKG software and provide an endpoint for the key generation ceremony
2. The backend bot initiates the DKG ceremony for a cluster. The node operators in the selected cluster participate and generate the deposit data and SSV keyshares
3. The backend bot deposits the deposit data to the Nexus Contract which further sends the data to the rollup bridge contract for validator activation
4. The backend bot deposits the keyshares data to the Nexus Contract which further sends it to the SSV contract for node operator registration
5. Whenever a validator needs to exit, the backend bot initiates the necessary exit process
