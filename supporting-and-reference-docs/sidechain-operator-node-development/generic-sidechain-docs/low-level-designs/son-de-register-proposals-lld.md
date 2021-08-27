# son-de-register-proposals-lld

## 1. Purpose

The purpose of this document is to provide low-level design for SON Deregister proposals by a witness and how we can extend these in the future.

## 2. Scope

The design requirement listed in this document will be limited to the node management of SONs.

The document will also outline the way we can use the proposals related to SON management in the future.

The document will also outline the new data structures required to validate, raise and approve proposals efficiently.

The document will only outline the design from the point where a SON is down for more than N Hours \( N = 12 Currently \).

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses.

As per the current implementation of Sidechain, a multi-sig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users.

Every Peerplays witnesses will have a bitcoin transaction signing key for this multi-sig bitcoin address and will be required to sign any withdrawal transaction.

When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multi-sig bitcoin address and the key of the incoming witness needs to be added.

The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. Graphene Proposals

All the info related to proposals can be found at [https://github.com/peerplays-network/peerplays/wiki/Proposal-Mechanism](https://github.com/peerplays-network/peerplays/wiki/Proposal-Mechanism)

## 5. Deregistering SON

According to the functional requirements of SON, if a SON is down for more than 12 hours, it should be deregistered from the network.

And for this to happen a witness has to raise a proposal that removes the SON from the network.

This has to happen if and only if there is no dependency on the SON for Bitcoin side operations. But this is out of the scope of this document.

All the statistics including the timestamp for when a SON is down is stored in son\_statistics\_object.

So when a witness detects that for any in\_maintenance SON the current\_time is greater by 12 hours than its last down timestamp, a witness proposal is raised.

The proposal will have a lifetime of 3 complete witness cycles so as to allow sufficient time for approval.

A new proposal object son\_proposal\_object\(And its indexes\) is introduced to fetch the proposals that are only related to SONs faster.

These proposals can even be used by other SONs in the future for raising proposals on the network.

i.e. The proposals can be used for Bitcoin-related proposals, to notify various events on the network, etc

```text
  A new type of operation can be created to suit the scenarios we want to handle and the operations can be put into the proposals for approval.
```

All the new data structures and helper functions can be found in the UML diagram below.

