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

# 🔐 Risks & Risk Mitigation

There are two major risks that the Nexus Network team foresees in the product adoption -

* **Security concerns -** The Nexus Network team takes security very carefully. It has built a minimal solution to reduce the security surface area -
  * The additions to the rollup bridge are minimal. They only add features for depositing to validators and claiming staking returns. The contracts have also been [audited](https://github.com/shellboxes/public-audit-reports/blob/main/reports/Nexus\_Network\_Security\_Audit\_Report.pdf).
  * The flow of funds is non-custodial
  * Even if Nexus contracts get compromised, exploiters can not access any funds as funds are either in the bridge contract or staked on Ethereum
* **Slashing risks -** The Nexus team has taken all necessary measures to reduce slashing risks and is working with the leading slashing insurance providers
* **Liquidity risks -** Nexus Network allows rollups to set a customizable staking limit to allow for regular withdrawals. Also, optimistic liquidity bridges can provide liquidity to withdrawing users in the worst case scenario

<figure><img src="../.gitbook/assets/Screenshot 2024-06-24 at 12.32.07 AM.png" alt=""><figcaption></figcaption></figure>



