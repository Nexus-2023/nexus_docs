# 🤖 Backend Bot

The backend bot monitors the Rollup bridge contracts and initiates the validator key creation process and validator exits.

### How does it work?

The backend bot monitors the Rollup bridge contracts and creates keys and exits depending on the staking limit set by the Rollup admin:

1. **When the current staking limit is less than the set staking limit:**  In this scenario following steps are executed;
   1. The backend bot calculates the number of validators to be created to reach the staking limit
   2. The validators are created and deposit data for validators is sent to the Nexus contract which sends it to the respective rollup bridge for activation
   3. The keyshares data is sent to the Nexus Contract which sends it to the SSV contract for registration, after which the Node operator can start performing validator duties
2. **When the current staking limit is more than the set staking limit**: In this scenario following steps are executed;
   1. The backend bot calculates the number of validators to be exited to reach the staking limit
   2. The exit message is created for the validators and is broadcast to the consensus chain
   3. Nexus contract is informed of all the validators for which exit is deposited. This is to ensure that the Nexus contracts can keep track of validators that are pending exit

_<mark style="color:blue;">The bot is deployed on AWS EC2 instance with limited access to the world. It can only be accessed by a single ssh key and that is stored offline and is only used for deployment.</mark>_

### Validator Key Management

Validator Key Management is a crucial part of any system. It can be done in two ways:

#### Centralized Key Management

Centralized keys require extra effort in storing the keys and mnemonic (if stored). Nexus Network manages the keys for a centralized system in the following ways:

1.  Whenever a new rollup is registered, a new mnemonic is created for them and stored in an AWS vault

    _<mark style="color:blue;">A new mnemonic is always encrypted using a salt and the salt is only passed when running the script so that it is stored in memory and nowhere else. A backup for the salt is stored offline</mark>_
2. Whenever new keys need to be created, the mnemonic is fetched from the AWS vault and used in the creation of keys. The nonce is stored in statefile and fetched from there.
3. After creating the keys, [SSV CLI](https://github.com/bloxapp/ssv-keys) is used to create validator keyshares
4. The keyshares and deposit data for validators are deposited to the Nexus Contract according to the logic detailed above
5. When the deposit is complete, the keys are destroyed as mnemonic and nonce are the only things needed to recreate keys or exits

_<mark style="color:blue;">The AWS vault is only accessible by the AWS instance on which the backend bot is running by IAM roles.</mark>_&#x20;

#### Decentralized Key Management (DKG)

To decentralize the key management system we would need to generate the key in a decentralized manner. For this, we use the [SSK DKG](https://docs.ssv.network/operator-user-guides/operator-node/enabling-dkg).&#x20;

Process for DKG:

1. Node Operators run the DKG software and provide an endpoint for the key generation ceremony
2. The backend bot initiates the DKG ceremony in which the node operators for the selected cluster participate and the deposit data and SSV Keyshare data are generated
3. The backend bot deposits the deposit data to the Nexus Contract which further sends the data to the rollup bridge contract for validator activation
4. The backend bot deposits the keyshare data to the Nexus Contract which further sends the data to the SSV contract for node operator registration&#x20;
5. Whenever a validator needs to exit the backend bot performs the necessary exit process

