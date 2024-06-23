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

# 🖥️ Integration Guide

exus Network has built an easy-to-integrate solution for the rollups. The Nexus contracts have already been deployed on Holesky. This document serves as an integration manual for rollup partners to test out the product by deploying a separate bridge contract. The goal of this exercise is to test the system end-to-end on Holesky, and over time deploy it on a public testnet.

Here are the steps to integrate with Nexus Network on the Goerli Test Network -

1. The Rollup selects the [preferred implementation](smart-contracts/nexus-bridge-eth/different-nexus-bridge-architecture.md) for the distribution of staking rewards\
   Code - [https://github.com/Nexus-2023/Nexus-Contracts/tree/main/contracts/nexus\_bridge](https://github.com/Nexus-2023/Nexus-Contracts/tree/main/contracts/nexus\_bridge)
2. Once the implementation is selected, the rollup has two deployment options to import the nexus package:
   1.  **Deploy Nexus as a library(**<mark style="color:green;">**Preferred**</mark>**)**: This can be done by deploying the nexus-package separately and storing the library address in the bridge contract

       Example code with polygon zkEVM: [https://github.com/Nexus-2023/zkevm-contracts/blob/polygon/Tangible/contracts/PolygonZkEVMBridge.sol](https://github.com/Nexus-2023/zkevm-contracts/blob/polygon/Tangible/contracts/PolygonZkEVMBridge.sol)
   2.  **Integrate with bridge**: This can be done by inheriting the nexus-package.

       Example code: [https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/demo\_contracts/BridgeContractDAO.sol](https://github.com/Nexus-2023/Nexus-Contracts/blob/main/contracts/demo\_contracts/BridgeContractDAO.sol)

&#x20;       Below is an Arbitrum Inbox Contract Bridge example:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

2. Share a public address with the Nexus team to get whitelisted. This address will act as the rollup admin address to trigger future parameter changes. The address can be a multi-sig, a contract address, etc
3. Once the address is whitelisted, perform a contact call to the [Nexus Network contracts](https://goerli.etherscan.io/address/0x7610dd2DE44aA3c03313b4c2812C482D86F3a9e7#writeProxyContract) highlighting -
   1. Rollup bridge contract address (This is the address to the newly deployed bridge after integrating the _`nexus-package`_)
   2. Staking limit for the rollup (percentage of ETH to be staked from the rollup bridge)
   3. ClusterID - Select the cluster of node operators to stake with (Over time this will become more customizable to allow the rollup to select multiple clusters and allocate a percentage of their assets to each cluster)
   4.  Nexus Fee- Select a fee for Nexus Network. It can be anything between 500(5%) to 1000(10%) .

       <figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
4. After finishing the whitelisting, validators will automatically get created by the Nexus off-chain bots
5. You can perform the following checks on the system -
   1. Fund the rollup bridge address - This should result in the creation of new validators. Validator activation takes a few hours
   2. Remove ETH from the bridge address - This should result in unstaking of ETH from the validators (note - unstaking on Holesky takes 2-3 days)
   3. Change staking ratio on the bridge - This should trigger staking/unstaking of ETH based on whether the ratio is increased/decreased. New validators are created if enough ETH is available or unstaked if enough ETH is not available to fulfill the ETH withdrawals

### Nexus Contracts (Holesky)

<table><thead><tr><th width="254">Name</th><th>Address</th></tr></thead><tbody><tr><td>Nexus Contract Proxy</td><td><a href="integration-guide.md#nexus-contracts-holesky">0xc0cb8f6c08AB23de6c2a73c49481FE112704F1b6</a></td></tr><tr><td>Nexus Contract Implementation</td><td><a href="https://holesky.etherscan.io/address/0xEa8213EB7C11bd537e60B2AbbcF298623D807C34#code">0xEa8213EB7C11bd537e60B2AbbcF298623D807C34</a></td></tr><tr><td>Validator Execution Reward Contract</td><td><a href="https://holesky.etherscan.io/address/0x1c29e5e0818b65911e32aC0ECDb494df389435bA#code">0x1c29e5e0818b65911e32aC0ECDb494df389435bA</a></td></tr><tr><td>Node Operator Contract Proxy</td><td><a href="https://holesky.etherscan.io/address/0x8Ff9f1C8b04F6Fa3ADD49081114209BF2a7671bc#code">0x8Ff9f1C8b04F6Fa3ADD49081114209BF2a7671bc</a></td></tr><tr><td>Node Operator Contract Implementation</td><td><a href="../">0xF03CEc7a46338Cbda9E98B17f3bafADAe54Fa09A</a></td></tr></tbody></table>

### Nexus Contracts (Goerli)(<mark style="color:red;">Depreciated</mark>)

<table><thead><tr><th width="254">Name</th><th>Address</th></tr></thead><tbody><tr><td>Nexus Contract Proxy</td><td><a href="https://goerli.etherscan.io/address/0x7610dd2DE44aA3c03313b4c2812C482D86F3a9e7">0x7610dd2DE44aA3c03313b4c2812C482D86F3a9e7</a></td></tr><tr><td>Nexus Contract Implementation</td><td><a href="https://goerli.etherscan.io/address/0x1bbb3ECa293450dF14fd3e270F1B6DBE2acAA8eB">0x1bbb3ECa293450dF14fd3e270F1B6DBE2acAA8eB</a></td></tr><tr><td>Validator Execution Reward Contract</td><td><a href="https://goerli.etherscan.io/address/0xc9DD08647269A1855858B6869350C219EB8761A7">0xc9DD08647269A1855858B6869350C219EB8761A7</a></td></tr><tr><td>Node Operator Contract Proxy</td><td><a href="https://goerli.etherscan.io/address/0x39F09e6FEB8163dBD5c46F2f30830bfaf896a12e">0x39F09e6FEB8163dBD5c46F2f30830bfaf896a12e</a></td></tr><tr><td>Node Operator Contract Implementation</td><td><a href="https://goerli.etherscan.io/address/0x07E7fa61D0c4d2aACF1Cc82E43b9de7A5Eb525D6">0x07E7fa61D0c4d2aACF1Cc82E43b9de7A5Eb525D6</a></td></tr></tbody></table>
