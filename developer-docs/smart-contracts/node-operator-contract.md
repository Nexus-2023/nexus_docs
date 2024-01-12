# Node Operator Contract

Node Operator Contract handles all the operations related to Node Operators. This contract is a UUPS upgradeable as a scoring mechanism needs to be added to it. It has the following functionalities:

1.  **Register SSV Operator:** This function registers a new Operator with Nexus Network

    ```solidity
    function registerSSVOperator(uint64 _operator_id, string calldata _pub_key, string calldata _ip_address, string calldata name)
    ```

    Parameters:

    1. **\_operator\_id**: Operator ID for node operator as registered with SSV network
    2. **\_pub\_key**: Operator public key for node operator as registered with SSV network
    3. **\_ip\_address**: DKG ip address for node operator that Nexus Network can use
    4. **name**: Name of the node operator
2.  **Update SSV operator IP**: This function updates the DKG IP for the node operator registered with Nexus

    ```solidity
    function updateSSVOperatorIP(uint64 _operator_id,string calldata _ip_address)
    ```
3.  **Add cluster:** This function creates a new cluster for specific operator that can be used for rollups to start staking

    ```solidity
    function addCluster(uint64[] calldata operatorIds,uint64 clusterId)
    ```
