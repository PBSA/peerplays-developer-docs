# son-wallet-list\_sons-lld

## 1. Purpose

The purpose of this document is to provide a low-level design for implementing list\_sons wallet functionality.

## 2. Scope

The design requirement listed in this document will be limited to the SON Wallet functionality.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses.

As per the current implementation of Sidechain, a multi-sig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users.

Every Peerplays witnesses will have a bitcoin transaction signing key for this multi-sig bitcoin address and will be required to sign any withdrawal transaction.

When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multi-sig bitcoin address and the key of the incoming witness needs to be added.

The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. Sidechain Operator Node \( SONs \)

More on SON Objects can be found at [SON Objects and Operators](https://peerplays.atlassian.net/wiki/spaces/PIX/pages/333971489/SON+Objects+and+Operators)

## 5. Proposed Design Change

Users should be able to list all the SON nodes on the blockchain.

They should be able to check the status of the SON which is provided by the son\_status variable in the son\_object.

They should be able to check the number of votes polled during the last cycle, which is provided by total\_votes in son\_object. This is changes in every maintenance interval.

For better UI, we can list the SONs based on their status i.e. Active SONs first, Inactive next, In\_Maintenance next in that order.

We can even further extend the types of data with the information stored in son\_statistics\_object as well.

This can be done by introducing a new structure \(son\_info in the UML below\) that can collate the information in database\_api and send it to the wallet.

