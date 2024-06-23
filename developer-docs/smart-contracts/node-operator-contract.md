# Node Operator Contract

The Node Operator Contract handles all the operations related to Node Operators including node operator registration, addition to clusters, and updation of operator details. This is an UUPS upgradeable contract that allows Nexus Network to add more functionalities for the node operators, including a performance scoring mechanism. It has the following functionalities:

1.  **Register SSV Operator:** This function registers a new SSV node operator with Nexus Network

    ```solidity
    function registerSSVOperator(uint64 _operatorId, string calldata _pubKey, string calldata _ipAddress, string calldata name)
    ```

    \
    Parameters:

    1. **\_operator\_id**: Operator ID for the node operator as registered with the SSV network
    2. **\_pub\_key**: Operator public key for the node operator as registered with the SSV network
    3. **\_ip\_address**: DKG IP address for node operator that Nexus Network can use
    4. **name**: Name of the node operator\

2.  **Update SSV operator IP**: This function updates the DKG IP for the node operator registered with Nexus

    ```solidity
    function updateSSVOperatorIP(uint64 _operatorId,string calldata _ipAddress)
    ```


3.  **Add cluster:** This function creates a new cluster by adding specific node operators, that can be used by rollups to start staking

    ```solidity
    function addCluster(uint64[] calldata operatorIds,uint64 clusterId)
    ```
