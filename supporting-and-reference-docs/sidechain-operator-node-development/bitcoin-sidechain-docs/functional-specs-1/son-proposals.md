# son-proposals

## 1. Purpose

The purpose of this document is to outline how SONs create and manage proposals that are created with various transactions

## 2. Scope

The functional requirements listed in this document cover the following proposals:

* son\_report\_down
* son\_report\_down\_operation
* son\_wallet\_update\_operation
* son\_wallet\_deposit\_create\_operation
* son\_wallet\_deposit\_process\_operation
* son\_wallet\_withdraw\_create\_operation
* son\_wallet\_withdraw\_process\_operation
* sidechain\_transaction\_create\_operation

## 3. Background

When certain types of transactions are performed by SONs, proposals are created to seek approval for these transactions. Proposals are created using create operation, and must be approved or rejected following proposal creation. Approved proposals fulfill their associated transaction \(for example deposit\), rejected proposals cancel the transaction.

## 4. Process Overview

N/A

## 5. Context

Each proposal contains a list of operations. These operations are the way to change anything in the blockchain, including performing transfers, modifying chain parameters, creating and modifying accounts, assets, and also proposals.

In addition to a list of operations, a proposal also has a defined review period and expiration time.

## 6. Flow Diagram

N/A

## **7. Proposals**

### **7.1 Proposal Lifetime**

A proposal must when **proposal\_create\_operation** operation is executed. This operation defines the review period and expiration time, as well as the list of operations proposed.

The proposal then must require approval via **proposal\_update\_operation**. This operation must include functions to add or remove active approvals, owner approvals, or key approvals.

Ability to delete \(which is effectively a veto on the proposal\) a proposal must exist via **proposal\_delete\_operation**. Execution of this operation must be restricted to accounts who are required authorities on this proposal, alternatively, the same effect would be achieved by simply not adding one's approval to the proposal.

### **7.2 Approving Proposals**

A proposal must be accepted when it reaches the required approvals through the **proposal\_update\_operation**. There is no specific step or operation to accept a proposal, instead authority must be checked on the **proposal\_update\_operation**, and upon successful verification, the proposed operations must be applied.

The list of authorities required for accepting a proposal must depend on the list of proposed operations. The proposal itself must not carry any extra authorities or requirements.

Specifically, the proposal-related operations must require the following authorities:

* the **proposal\_create\_operation** requires only the authority of the submitter
* the **proposal\_update\_operation** which adds an approval requires the authority of the approver
* the **proposal\_delete\_operation** requires the authority of the veto-giver, which must be a required authority on at least one operation of the affected proposal.

Other operations have different authority requirements.

### **7.2 Types of Approvals**

There are three kinds of approvals that must be tracked for each operation: owner approvals, active approvals, and key approvals. Each operation must specify its required approvals of each kind separately.

### **7.3 Fees**

Fees must be associated with operations, not proposals. However, the operations to create or update a proposal must carry their own fees, for which the payer must be chosen when creating the operation. Each of these transactions must carry a 20 unit fee plus a price per kilobyte.

The fees for the operations inside the proposal must be determined by operation. For Peerplays-related ones, like creating a sport, the fee is fixed to 1 unit \(equal to GRAPHENE\_BLOCKCHAIN\_PRECISION\) and must be paid by the witness account.

### **7.4 Operations requiring proposals**

List of operations and their associated proposals:

* **SON report down \(son\_report\_down\)** must be checked for timestamps stored in SON statistics object to confirm that SON is down \(missed two heartbeats\)
* **SON De-register \(son\_delete\_operation\)** must be checked for total downtime stored in SON statistics objects to confirm that SON has been in maintenance for too long and must be de-registered/deleted
* **SON wallet update \(son\_wallet\_update\_operation\)** must be checked against active SONs to find matching transaction data. 2/3rds of active sons must confirm a match for a successful check
* **Deposit \(son\_wallet\_deposit\_create\_operation**\) must not required proposal for object creation, but must be checked to verify that parameters are correct \(same as in existing object\), an active SON is creating the object, and only the expected son is confirming the transaction.
* **Process Deposit \(son\_wallet\_deposit\_process\_operation\) +asset\_issue\_**operation - check critical values from son\_wallet\_deposit\_object are identical to critical values from sidechain transaction. Minimum mandatory checks are: check sender and amount. Depending on sidechain, checks of other values may be warranted \(Bitcoin - vout and number of transaction confirmations, Peerplays - exchange rate\)
* **Transfer \(transfer\_operation\)** - TBD
* **Withdrawal \(son\_wallet\_withdraw\_create\_operation\)** must not require a proposal for object creation, however, must be checked to verify that parameters are correct \(same as in existing object\), an active SON is creating the object, and only the expected son is confirming the transaction
* **Process Withdrawal \(son\_wallet\_withdraw\_process\_operation\) + asset\_reserve\_operation** - check critical values from son\_wallet\_withdraw\_object are identical to critical values from sidechain transaction. Minimum mandatory checks are: check sender and amount. Depending on sidechain, checks of other values may be warranted \(Bitcoin - vout and number of transaction confirmations, Peerplays - exchange rate\)
* **Sidechain transaction creation \(sidechain\_transaction\_create\_operation\)** must be checked to verify that given object id is already created

## 8 Reference

List of Graphene operations

[https://github.com/peerplays-network/peerplays/blob/master/libraries/chain/include/graphene/chain/protocol/operations.hpp\#L55](https://github.com/peerplays-network/peerplays/blob/master/libraries/chain/include/graphene/chain/protocol/operations.hpp#L55)

