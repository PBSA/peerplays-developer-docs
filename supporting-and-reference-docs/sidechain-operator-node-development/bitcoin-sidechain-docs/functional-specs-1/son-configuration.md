# son-configuration

## 1. Purpose

The purpose of this document is to outline the steps required by a Peerplays node operator to enable/disable Sidechain plugin in the Peerplays blockchain.

## 2. Scope

The functional requirement listed in this document will be limited to the configuration portion of the SON. This will outline the required steps that will be performed by the Peerplays node operator to enable/disable Sidechain plugin in the Peerplays blockchain from the command line. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of SONs. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every SON will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a SONs changes, the transaction signing key of the outgoing SON needs to be removed from the multisig bitcoin wallet and the key of the incoming SON needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes (SONs).

## 4. Process Overview

Described here is the process to configure the SON plugin in the Peerplays blockchain.

## 5. Context

The Peerplays blockchain will allow the node operators to enable/disable the Sidechain functionality on their node. They will have to stake PPY 50 to be able to register as an SON. As soon as the Sidechain functionality is enabled/disabled and the user vests 50 PPY, the Peerplays blockchain will add the node to its list of Sidechain enabled nodes and make the node operator available for SON voting. An SON enabled node will be able to operate as an SON only when it receives the votes required to become one of the top 15 SONs.

## 6. Flow Diagram

## **7. SON Commands**

Any Peerplays node can become a SON irrespective of it is a witness or not. It can connect to a local or remote Bitcoin node and verify the transactions.

### **7.1 Enable SON**

./programs/witness\_node/witness\_node --resync --replay --son-enable

This command will enable the SON functionality in the Peerplays node. The node will be registered in the peerplays blockchain as an SON enabled node. Peerplays users will be able to vote for this user to be an active SON. The node will be able to listen to the changes on the Bitcoin blockchain. An active SON can perform all the functionality of the sidechain once it has been voted into the top 15 SONs.

#### **7.1.1. Pre-Conditions & Errors**

The error messages are to be displayed in GUI wallet or cli\_wallet or other programatic means.

1. The SON node should not be enabled and active for the same Peerplays account
2. Trying to enable an already in enabled SON node will give the error message "SON is already enabled for the account\_name."
3. The account trying to enable SON should have a minimum balance of 50 PPY which has to be VESTED to become SON
4. If the account is not having the minimum required balance, an error message "Minimum VESTING Balance for SON node activation is not available. Make sure that the account is funded with MINIMUM\_VESTING\_BALANCE"
5. If a previous VESTED balance is available, it will not be considered to be the minimum VESTING BALANCE. This is to avoid accounts spamming the blockchain with SON-activate commands.

### **7.2. Disable SON**

This section talks about permanently disabling SON.

./programs/witness\_node/witness\_node --resync --replay --son-disable

This command will disable the SON functionality in the Peerplays node and deregister the node in the peerplays blockchain as an SON enabled node. Any votes casted to the user to be an active SON will be reverted. The user will be able to claim their vested PPY after 2 days of deregistering their node.

#### **7.1.2. Pre-Conditions & Errors**

The error messages are to be displayed in GUI wallet or cli\_wallet or other programatic means.

1. Only a SON node which is in "enabled state" can be disabled
2. Trying to de-activate/disable a Peerplays node with SON not enabled will give the error message "SON is not enabled for account\_name"&#x20;
3. If a SON is disabled and trying to move the VESTED funds before 2 days duration via any means should be prevented

## API Requirements

All the v1 Sidechain APIs should work as it is. Except for those, an API to provide the list of SON enabled users will be required. The API should return the list in the following format:

Expand source

\[

{

serial\_num: 1,

id: 1.20.1,

user: memphis123,

rank: 1,

about\_url: www.memphis-node.com,

sidechain\_url: http://www.memphis-node.com/sidechain,

is\_active: true,

votes: 23223245

},

{

....

},

....

]

## CLI Wallet Command Requirements

These are new options to be added.

1. list\_sons: We need a command in the CLI Wallet similar to list\_witnesses to get the list of SONs.
2. list\_active\_sons: This command should provide a list of active SONs on the Peerplays blockchain.
