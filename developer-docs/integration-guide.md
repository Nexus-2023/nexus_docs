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

# 🖥 For Developers : Quick Guide

Nexus Network has built an easy-to-integrate solution for the rollups. The Nexus contracts have already been deployed on Goerli. This document serves as an integration manual for rollup partners to test out the product by deploying a separate bridge contract. The goal of this exercise is to test the system end-to-end on Goerli, and over time deploy it on a public testnet.

Here are the steps to integrate with Nexus Network on the Goerli Test Network -

1.  Import the _`nexus-package`_ to your bridge contract

    Code - [https://github.com/Nexus-2023/Nexus-Contracts/tree/nexus/withdrawal/contracts/nexus\_bridge](https://github.com/Nexus-2023/Nexus-Contracts/tree/nexus/withdrawal/contracts/nexus\_bridge)

    \
    Polygon bridge:

<div>

<img src="https://prod-files-secure.s3.us-west-2.amazonaws.com/36d96375-0fd3-48dd-b3d7-8bc71c8663d5/af1be306-ea09-4b0b-bcb0-b3987d3361d0/Untitled.png" alt="Untitled">

 

<figure><img src="../.gitbook/assets/Untitled (3).png" alt=""><figcaption></figcaption></figure>

</div>

&#x20;       Optimism Bridge:

<div>

<img src="https://prod-files-secure.s3.us-west-2.amazonaws.com/36d96375-0fd3-48dd-b3d7-8bc71c8663d5/8b98f318-9750-4efe-b127-ac1faa83cd2d/Untitled.png" alt="Untitled">

 

<figure><img src="../.gitbook/assets/Untitled (4).png" alt=""><figcaption></figcaption></figure>

</div>

2. Share a public address with the Nexus team to get whitelisted. This address will act as the rollup admin address to trigger future parameter changes. The address can be a multi-sig, a contract address, etc
3.  Once the address is whitelisted, perform a contact call to the [Nexus Network contracts](https://goerli.etherscan.io/address/0xd1c788ac548cb467b3c4b14cf1793bca3c1dcbeb#writeProxyContract) highlighting -

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
