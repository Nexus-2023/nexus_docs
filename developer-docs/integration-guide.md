---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🖥 Integration Guide

Nexus Network has built an easy-to-integrate solution for the rollups. The Nexus contracts have already been deployed on Goerli. This document serves as an integration manual for rollup partners to test out the product by deploying a separate bridge contract. The goal of this exercise is to test the system end-to-end on Goerli, and over time deploy it on a public testnet.

Here are the steps to integrate with Nexus Network on the Goerli Test Network -

1.  There are  different types of nexus bridge contracts that you can implement in the bridge contract:

    Code - [https://github.com/Nexus-2023/Nexus-Contracts/tree/main/contracts/nexus\_bridge](https://github.com/Nexus-2023/Nexus-Contracts/tree/main/contracts/nexus\_bridge)

    After selecting the type of nexus bridge you need to figure out how you want to integrate:

    1.  **Deploy Nexus as a library**: This can be done by deploying the Nexus Library separately and storing the library address in the bridge contract

        Example code with polygon zkEVM: [https://github.com/Nexus-2023/zkevm-contracts/blob/polygon/Tangible/contracts/PolygonZkEVMBridge.sol](https://github.com/Nexus-2023/zkevm-contracts/blob/polygon/Tangible/contracts/PolygonZkEVMBridge.sol)
    2.  **Integrate with bridge**: This can be done by inheriting the Nexus contract.

        Example code: [https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/demo\_contracts/BridgeContractDAO.sol](https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/demo\_contracts/BridgeContractDAO.sol)

&#x20;       Optimism Bridge:

<div>

<img src="https://prod-files-secure.s3.us-west-2.amazonaws.com/36d96375-0fd3-48dd-b3d7-8bc71c8663d5/8b98f318-9750-4efe-b127-ac1faa83cd2d/Untitled.png" alt="Untitled">

 

<figure><img src="../.gitbook/assets/Untitled (4).png" alt=""><figcaption></figcaption></figure>

</div>

2. Share a public address with the Nexus team to get whitelisted. This address will act as the rollup admin address to trigger future parameter changes. The address can be a multi-sig, a contract address, etc
3.  Once the address is whitelisted, perform a contact call to the [Nexus Network contracts](https://goerli.etherscan.io/address/0x7610dd2DE44aA3c03313b4c2812C482D86F3a9e7#writeProxyContract) highlighting -

    1. Rollup bridge contract address (This is the address to the newly deployed bridge after integrating the _`nexus-package`_)
    2. Staking limit for the rollup (percentage of ETH to be staked from the rollup bridge)
    3. ClusterID - Select the cluster of node operators to stake with (Over time this will become more customizable to allow the rollup to select multiple clusters and allocate a percentage of their assets to each rollup)
       1. For devnet choose 1

    <div>

    <img src="https://prod-files-secure.s3.us-west-2.amazonaws.com/36d96375-0fd3-48dd-b3d7-8bc71c8663d5/6909a792-109c-48de-a52c-bae14fed3865/Untitled.png" alt="Untitled">

     

    <figure><img src="../.gitbook/assets/Untitled (5).png" alt=""><figcaption></figcaption></figure>

    </div>
4. After finishing the whitelisting, validators will automatically get created by the Nexus off-chain bots
5. You can perform the following checks on the system -
   1. Fund the rollup bridge address - This should result in the creation of new validators. Validator activation takes 2-3 days
   2. Remove ETH from the bridge address - This should result in unstaking of ETH from the validators (note - Unstaking on Goerli takes 2-3 days)
   3. Change staking ratio on the bridge - This should trigger staking/unstaking of ETH based on whether the ratio is increased/decreased. New validators are created if enough ETH is available or unstaked if enough ETH is not available to fulfill the ETH withdrawals

### Nexus Contracts (Goerli)

<table><thead><tr><th width="254">Name</th><th>Address</th></tr></thead><tbody><tr><td>Nexus Contract Proxy</td><td><a href="https://goerli.etherscan.io/address/0x7610dd2DE44aA3c03313b4c2812C482D86F3a9e7">0x7610dd2DE44aA3c03313b4c2812C482D86F3a9e7</a></td></tr><tr><td>Nexus Contract Implementation</td><td><a href="https://goerli.etherscan.io/address/0x1bbb3ECa293450dF14fd3e270F1B6DBE2acAA8eB">0x1bbb3ECa293450dF14fd3e270F1B6DBE2acAA8eB</a></td></tr><tr><td>Validator Execution Reward Contract</td><td><a href="https://goerli.etherscan.io/address/0xc9DD08647269A1855858B6869350C219EB8761A7">0xc9DD08647269A1855858B6869350C219EB8761A7</a></td></tr><tr><td>Node Operator Contract Proxy</td><td><a href="https://goerli.etherscan.io/address/0x39F09e6FEB8163dBD5c46F2f30830bfaf896a12e">0x39F09e6FEB8163dBD5c46F2f30830bfaf896a12e</a></td></tr><tr><td>Node Operator Contract Implementation</td><td><a href="https://goerli.etherscan.io/address/0x07E7fa61D0c4d2aACF1Cc82E43b9de7A5Eb525D6">0x07E7fa61D0c4d2aACF1Cc82E43b9de7A5Eb525D6</a></td></tr></tbody></table>
