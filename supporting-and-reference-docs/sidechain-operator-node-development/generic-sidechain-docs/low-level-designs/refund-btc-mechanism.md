# refund-btc-mechanism

## 1. Purpose

The purpose of this document is to outline the steps required by a witness to create a proposal to refund the BTC deposited when more than 1/3rd SONs were down/inactive.

## 2. Scope

The design requirement listed in this document will be **limited to the refund deposited BTC functionality of the SON – other coins should be handled differently if needed**. This will outline the required steps that will be performed by the witness nodes to create a proposal to refund the BTC in the Peerplays blockchain. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses. As per the current implementation of Sidechain, a multisig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every Peerplays witnesses will have a bitcoin transaction signing key for this multisig bitcoin address and will be required to sign any withdrawal transaction. When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multisig bitcoin address and the key of the incoming witness needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\). The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. Process Overview

Described here is the design for refunding the BTC that was deposited on the Bitcoin Blockchain when more than 1/3rd SONs were inactive in the Peerplays blockchain.

## 5. Context

There can be a situation where more than 1/3rd of the SONs are down and a user deposits BTC to his account but it's not credited to his account. The ideal way to deal with such a situation is to create smart contracts \(HTLC\) to refund the BTC back to the user. In the absence of smart contracts, the below mentioned implementations have been suggested.

## 6. Implementations Suggested

1. This can be handled by the witness nodes. Witnesses can't sign any transaction related to BTC sidechain since that is the responsibility of the SONs but they should be able to create a proposal to refund the BTC when the SONs were down and the SONs should be able to ratify this proposal once they are online. This way, the BTC of the users who deposited it when the SONs were down won't be lost and instead refunded. This proposal can be created on-chain or manually by the witnesses.  a. We just need to The implementation for a manual proposal by witnesses will be much lower than any other implementation suggested above. b. In an on-chain implementation, the witnesses will have to run a bitcoin instance on their node and the peerplays code will include a bitcoin listener \(already implemented in sidechain\) which will listen to the changes on the bitcoin blockchain and will keep track of SONs' inactivity. As soon as more than 1/3rd of the SONs become inactive, it will check for any deposits made to the Bitcoin wallet and create a proposal to refund it. This proposal can be signed by the SONs and the amount can be refunded.
2. During the period when more than 1/3rd of the SONs are down, the active SONs should either create the proposal for depositing pBTC to the users or to simply refund the BTC deposited by the user and when the rest of the SONs become active again, they should be able to verify it and sign the proposal. In this case, the time taken for the depositing the pBTC to the user's account may be more than 3 hours.
3. When the SONs become active after a period of inactivity, they should first check for any missed transactions on the Bitcoin blockchain and refund the BTC to the user that was deposited during the inactivity.

## 7. Flow Diagram

To be done on the basis of the implementation chosen.

