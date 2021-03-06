# btc-refunds

## 1. Purpose

The purpose of this document is to outline the requirements for a refund process that may be initiated by the user to refund a transaction that has not been completed \(not reflected in Primary Wallet\).

## 2. Scope

The design requirement listed in this document will be **limited to the refund deposited BTC functionality of the SON – other coins should be handled differently if needed**. This will outline the required steps that will be performed to refund the BTC in the Peerplays blockchain. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of SONs. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every SON will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a SONs changes, the transaction signing key of the outgoing SON needs to be removed from the multisig bitcoin wallet and the key of the incoming SON needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\)

## 4. Process Overview

Process overview below details typical steps in identifying and handling a BTC transaction that does not complete because required number of active SONs check fails. This causes transactions to wait until active SON threshold is met.

Note that this process is transaction agnostic. For details surrounding specific transactions, check BTC Deposit and BTC Withdrawal requirements

Steps involved:

1. User initiates a BTC transaction \(such as deposit or withdrawal\)
2. Listener identifies that a BTC transaction is initiated
3. Listener passes event data to handler
4. Handler receives event data and creates an object \(normally SON Wallet Object, SON Wallet Deposit Object or SON Wallet Withdraw Object\)
5. Object is checked by by all active SONs to compare object data from the chain against data generated by each SON.
   1. If data does not match, transaction is terminated
   2. If data matches, object is deemed ‘Confirmed'. Proceed to next step
6. Handler checks the number of active SONs
   1. If active SONs are &gt; 5, proceed to next step
   2. If active SONs are &lt; 5, store transaction for processing until required number of active SONs is available \(this step will be repeated until SON availability requirement is met\)
7. User becomes aware that transaction didn't complete by checking balance, then they copy transaction id of the transaction that's stored for processing \(transaction that is going to be refunded\). Transactions are refundable until sons move funds inside the Primary Wallet.
8. User creates another transaction using transaction id of transaction they want to refund to move funds to their own address
9. User signs the transaction using a private key that matches the public key they provided in sidechain address mapping
10. Transaction is pushed to bitcoin network and user is refunded once transaction is processed

Note that while this example process uses insufficient number of active SONs as failure reasons, there may be other reasons why transaction was not processed.

## 5. Context

Refund scenario is initiated by the user in relation to BTC transactions that fail to process. Failure may be caused by various scenarios, for example when there are less than 5 active SONs, transaction will wait until 5 or more SONs become available and remain unprocessed until minimum active SON condition is met.

Until SONs complete the transaction and it is reflected in Primary Wallet, user may initiate a refund by creating another BTC transaction that uses transaction id of the original transaction \(the one user wants to refund\).

Note that in all cases, refunds are not issued automatically and must be initiated by the user.

## 6. Specification

### 6. 1 Initiating refunds

System does not automatically initiate refunds because when active SONs threshold check fails, transaction is stored with intent to be processed when sufficient number of active SONs is available.

User must initiate the refund themselves by initiating a transaction that includes transaction id of the transaction that needs to be refunded, signs the transaction using a private key that matches the public key they provided in sidechain address mapping. Transaction is then pushed to bitcoin network and user is refunded once transaction is processed

### 6. 2 Implementation method - One-or-weighted-multisig

One-or-weighted-multisig method of deposit implementation allows to send funds from this address with 2/3 weights of SON votes \(like in Primary Wallet\) or with single user signature. To create such address we need:

1. user public key
2. all SONs public keys
3. every SON weight

When funds are being sent from this address, system must first check if user signature is correct. When signature is correct, system completes the transaction. Otherwise, when signature is incorrect, system checks for 2/3 weights of SON votes to complete the transaction.

## 7. Flow Diagram

N/A

## Related Documents

[https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/functional-specification-bitcoin-deposit-handling](https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/functional-specification-bitcoin-deposit-handling)

[https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/functional-specifications-bitcoin-withdrawal](https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/functional-specifications-bitcoin-withdrawal)

[https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/functional-specifications-btc-transaction-signing](https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/functional-specifications-btc-transaction-signing)

[https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/son-multisig-bitcoin](https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/son-multisig-bitcoin)

[https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/son-multisig-bitcoin](https://app.gitbook.com/@peerplays/s/community-project-docs/son/functional-pecs/son-multisig-bitcoin)

