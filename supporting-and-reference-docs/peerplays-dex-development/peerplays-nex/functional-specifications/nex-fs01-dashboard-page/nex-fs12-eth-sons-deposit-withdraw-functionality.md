---
description: >-
  The functional requirement for Peerplays NEX application to explain the ETH
  deposit/withdraw functionality.
---

# NEX-FS12 ETH-SONs Deposit/Withdraw Functionality

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the inclusion of **ETH** as one of the asset in the existing NEX deposit/withdraw functionality in the Dashboard page. The document explains the functionality from a business and user perspective.&#x20;

## 2. Document Tracking

### 2.1 Parent Document

This document is a child document of NEX Dashboard page (NEX-FS01).

### 2.2 Categorization

This document relates to the following tags,\
\
****`App component`

`process`

`page`&#x20;

## 3. Scope

This FS will describe the requirements and basic design for the app’s ETH withdraw/deposit design and functions.

### 3.1 Components

Specific components and features covered in this FS include:

1. Dashboard ETH Deposit function
2. Dashboard ETH Withdraw function

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code        | Meaning                                              |
| ----------- | ---------------------------------------------------- |
| NEX-FS012-# | NEX App Requirement - ETH Deposit/Withdraw Functions |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

## 5. Process Overview

The following process will be described here as follows,

* ETH Deposit
* ETH Withdraw

In this process overview, the example is considered as the authenticated user using the application  and has an appropriate balance in wallet to perform the transactions.

### 5.1 ETH Deposit

1. The user navigates to the Dashboard page's **Deposit** tab to begin the deposit function.
2. The select asset option has a drop-down list to select the desired asset.
3. The user selects **ETH** from the list of asset available for deposit.&#x20;
4. The page loads instruction for initiating a ETH deposit to the user's app account.
5. In this case, [**Metamask**](nex-fs12-eth-sons-deposit-withdraw-functionality.md#8.2-metamask-connection) can be used as an external application to perform the ETH deposit.
6. User should login the Metamask wallet, from where the ETH asset should be transferred to the user app's wallet.
7. When the user returns to the app, message will be notified after the successful ETH deposit into the account
8.  Other methods to deposit ETH into the wallet are described below,



    * [ ] Transfer ETH to smart contract address with the help of `curl`



    {% code overflow="wrap" lineNumbers="true" %}
    ```json
    curl http://10.11.12.202:8545 -X POST -H "Content-Type: application/json" --data '{"method":"eth_sendTransaction","params":[ { "from": "0x5c79a9f5767e3c1b926f963fa24e21d8a04289ae", "to": "0x572eA65762bFFf521C11Ce5334FfaaF4bDD4974e", "value": "0x1BC16D674EC80000" } ],"id":1,"jsonrpc":"2.0"}'
    ```
    {% endcode %}



    * [ ] Transfer ETH using `geth`



    {% code overflow="wrap" lineNumbers="true" %}
    ```json
    > personal.unlockAccount("0x5c79a9f5767e3c1b926f963fa24e21d8a04289ae", "", 1200)
    > eth.sendTransaction({from: "0x5c79a9f5767e3c1b926f963fa24e21d8a04289ae",to: "0x572eA65762bFFf521C11Ce5334FfaaF4bDD4974e", value: "5000000000000000000"})
    ```
    {% endcode %}

### 5.2 ETH Withdraw

1. The user navigates to the dashboard page’s **Withdraw** tab to begin the withdraw function.
2. The select asset option has a drop-down list to select the desired asset.
3. The user shall selects **ETH** asset from a list of assets available for withdraw.
4. The page loads the instruction for initiating a ETH withdraw from use app's account.
5. The user reviews the ETH balance in their wallet and enter the required amount of ETH to withdraw.
6. The user should enter a valid son-account address to which they wish to receive the ETH asset.
7. The app should check the account name and ensure its validity.
8. The user should review the withdraw information and click to confirm the withdraw.
9. The app processes and submit the withdraw request to son-account.
10. A notification message shall be displayed to indicate the successful ETH withdraw.
11. The app page will load to reflect the new available balance and any other changes after the withdraw.
12. The command line to withdraw ETH is explained below,

<pre class="language-json" data-overflow="wrap"><code class="lang-json"><strong>## The following command can be used to withdraw ETH to son-account
</strong><strong>transfer account01 son-account 2 ETH "" true
</strong></code></pre>

## 6. Context

The existing dashboard page holds many of the most used functions of the app to facilitate ease of use and it can be used as the home page. This document include the functionality for ETH deposit and withdraw along with the existing assets in dashboard page.

## 7. Design Wire-frames

The wire-frame design explain the representation of deposit and withdraw of existing assets.

**Deposit Asset:** [https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

**Withdraw Asset:** [https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/e0b637fc-73ee-4b45-80b4-03bace4df74f/](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/e0b637fc-73ee-4b45-80b4-03bace4df74f/)&#x20;

The same design can be used to represent **ETH deposit/withdraw** into the dashboard.

The overall dashboard designs are available in the below location,\
[https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

## 8. Requirements

The requirement specific to ETH deposit and withdraw are explained below,

The [Dashboard page layout](https://devs.peerplays.com/supporting-and-reference-docs/peerplays-dex-development/peerplays-nex/functional-specifications/nex-fs01-dashboard-page#8.1.-dashboard-page-layout), [Asset Deposit function](https://devs.peerplays.com/supporting-and-reference-docs/peerplays-dex-development/peerplays-nex/functional-specifications/nex-fs01-dashboard-page#8.2.-dashboard-asset-deposit-functions), [Asset withdraw function](https://devs.peerplays.com/supporting-and-reference-docs/peerplays-dex-development/peerplays-nex/functional-specifications/nex-fs01-dashboard-page#8.3.-dashboard-asset-withdrawal-functions), [Asset swap function](https://devs.peerplays.com/supporting-and-reference-docs/peerplays-dex-development/peerplays-nex/functional-specifications/nex-fs01-dashboard-page#8.4.-dashboard-asset-swap-functions) are the modules in the Dashboard. Click on each topic to learn in details about its requirement.

### 8.1 ETH Deposit

If ETH asset is selected in the asset deposit function,

**NEX-FS12-01:** shall display the ETH icon in the deposit instructions.

**NEX-FS12-02:** shall display steps to deposit asset into wallet.

**NEX-FS12-03:** Shall display the button "Connect Metamask", which on-click directs to MetaMask wallet. From the wallet, ETH asset can be transferred to the User app's wallet. (Only After successful MetaMask connection)

### 8.2 MetaMask Connection

&#x20;To perform the ETH deposit, Metamask connection is required.

**NEX-FS12-04:** shall provide a button named "Connect MetaMask" with **arrow** which directs to MetaMask wallet to establish connection.

**NEX-FS12-05:** shall provide an option to input credentials to login into MetaMask wallet.

**NEX-FS12-06:** shall display check mark in the button when MetaMask is connected and&#x20;

**NEX-FS12-07:** shall gray out the button after connection establishment.

{% hint style="info" %}
The Peerplays user account's son information will be updated with the new Ethereum account information
{% endhint %}

**NEX-FS12-08:** shall provide a link at the bottom of the card which helps to download MetaMask.

### 8.3 ETH Withdraw

If ETH asset is selected in the asset withdraw function,

**NEX-FS12-09:** shall display the ETH icon in the withdraw function with available asset for withdraw.

**NEX-FS12-10:** shall provide an option to input "**withdraw address**" to login into Peerplays account

**NEX-FS12-11:** shall provide an option to input son-account address in case of ETH asset selection.

**NEX-FS12-12:** shall provide the details about estimated Fees, Total transaction, confirmation time, etc., based on the amount of ETH asset chosen for withdraw.

**NEX-FS12-13:** shall provide a button to  withdraw ETH after successful validation of account address.

## 9. Appendix A: Glossary

|          |                                                                               |
| -------- | ----------------------------------------------------------------------------- |
| FS       | Functional Specification                                                      |
| ETH      | Ethereum Asset                                                                |
| MetaMask | Third-party application used in the deposit of ETH asset to user application. |
