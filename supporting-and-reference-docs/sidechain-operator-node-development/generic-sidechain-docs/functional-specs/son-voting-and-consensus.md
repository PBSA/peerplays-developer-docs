# SON Voting and Consensus

## Purpose

The purpose of this document is to outline the steps required by a user to vote for an SON in the Peerplays blockchain and the consensus mechanism for SONs.

## Scope

The functional requirement listed in this document will be limited to the voting and consensus portion of the SON. This will outline the required steps that will be performed by a user to vote for an SON in the Peerplays blockchain and their selection as active SONs. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of SONs. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every Peerplays witnesses will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a SONs changes, the transaction signing key of the outgoing witness needs to be removed from the multisig bitcoin wallet and the key of the incoming witness needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\). The SONs will be independent of the witnesses and don't need to be changed much often.

## Process Overview

Described here is the process to vote for SONs in the Peerplays Blockchain.

## Context

The Peerplays blockchain will allow the users to vote for the SON enabled node operators. The Peerplays blockchain should have a minimum of 15 active SON nodes. The user should be able to see a list of the SON enabled nodes and vote for up to 15 nodes. An SON enabled node will be able to operate as an active SON only when it receives the votes required to become one of the top 15 SONs. The number of active SON Nodes can be increased by the witnesses/advisor by making a change to the chain parameters. The active SONs will have corresponding bitcoin transaction signing keys in order to sign transactions for the Peerplays bitcoin multisig address on the bitcoin blockchain. When the active SONs change on the Peerplays blockchain, their corresponding bitcoin transaction signing keys are changed in the Peerplays bitcoin multisig address on the bitcoin blockchain. 

Each SON when processing inter blockchain communications \(IBC\) will carry a percentage of weight in the signing based on the votes they have received. Every IBC request will require signed consensus by at least ⅔ of the Active Cast Weighted \(ACW\) Votes for the SONs. Active Cast Weighted votes means the votes casted to active SONs by users who have staked some PPY weighted by their staked PPY.

If an SON enabled node's downtime exceeds 12 continuous hours, it should be automatically deregistered in the Peerplays blockchain as an SON enabled node and all the votes cast for me should be reverted.

If an SON decides to stop operating the node by disabling the SON plugin, any votes cast to him will be removed and another SON will take his place.

## Flow Diagram

![](../../../../.gitbook/assets/0.png)

## SON Voting Command

The below command from the CLI wallet will be used to vote for SONs:  
**vote\_for\_son** &lt;voting\_account&gt; &lt;son&gt; &lt;approve&gt; &lt;broadcast&gt;

* voting\_account\_id: the name or id of the account who is voting with their shares 
* son: the name or id of the son's owner account
* approve: true if you wish to vote in favor of that son, false to remove your vote in favor of that son
* broadcast:  true if you wish to broadcast the transaction

