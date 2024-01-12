# 🤖 Backend Bot

The backend bot monitors the Rollup bridge contracts and initiates the validator key creation process and validator exits.

### How does it work?

The backend bot monitors the Rollup bridge contracts and creates keys and exits depending on the staking limit set by the Rollup admin:

1. **When the current staking limit is less than the set staking limit:**  In this scenario following steps are executed;
   1. The backend bot calculates how many validator needs to be created so that the staking limit reaches the set limit
   2. The validators are created and deposit data for validators is sent to the Nexus contract which sends it to the respective rollup bridge for activation.
   3. The keyshare data is sent to the nexus contract which sends it to the SSV contract so that the Node operator can start performing validator duties
2. **When the current staking limit is more than the set staking limit**: In this scenario following steps are executed;
   1. The backend bot calculates how many validators need to be exited for the staking limit to reach the set limit
   2. The exit message is created for the validators and broadcast to the consensus chain
   3. Nexus contract is informed of all the validators for which exit is deposited. This is to ensure that the nexus can keep track of validators in exiting mode

_<mark style="color:blue;">The bot is deployed on AWS EC2 instance with limited access to the world. It can only be accessed by a single ssh key and that is stored offline and is only used for deployment.</mark>_

### Validator Key Management

Validator Key Management is a crucial part of any system. It can be done in two ways:

#### Centralized Key Management

Centralized keys require extra effort in storing the keys and mnemonic(if stored). Nexus maanges keys for centralized system in following ways:

1.  Whenever a new Rollup is registered a new mnemonic is created for them and stored in AWS vault

    _<mark style="color:blue;">New mnemonic is always encrypted using a salt and the salt is only passed when running the script so that it is stored in memory and nowhere else. A backup for the salt is stored offline.</mark>_
2. Whenever new keys need to be created, mnemonic is fetched from AWS vault and used in creation of keys, the nonce is stored in statefile and fetched from there.
3. After creating the keys, [SSV CLI](https://github.com/bloxapp/ssv-keys) is used to create validator keyshares
4. The keyshares and deposit data for validators are deposited to the Nexus Contract according to the logic mentioned above
5. When the deposit is complete the keys are destroyed as mnemonic and nonce is only thing needed to recreate keys or exits

_<mark style="color:blue;">The AWS vault is only accessible by the AWS instance on which the backend bot is running by IAM roles.</mark>_&#x20;

#### Decentralized Key Management(DKG)

To decentralize the key management system we would need to generate the key in a decentralized manner. For this, we use the [SSK DKG](https://docs.ssv.network/operator-user-guides/operator-node/enabling-dkg).&#x20;

Process for DKG:

1. Node Operator should run the DKG software and should provide an endpoint for the key generation ceremony
2. The backend bot will initiate the DKG ceremony in which the node operators will participate and the deposit data and SSV Keyshare data will be generated
3. The backend bot then deposits the  deposit data to the Nexus Contract which further sends the data to the Rollup bridge contract for validator activation
4. The backend bot then deposits the keyshare data to the Nexus Contract which further sends the data to the SSV contract for Node operators.&#x20;
5. Whenever a validator needs to exit the backend bot performs the necessary exit process.

