# son-rewards

## **1. Purpose**

The purpose of this document is to outline the steps required to pay the SONs for their contribution to the Peerplays network.

## **2. Scope**

The functional requirement listed in this document will be limited to the payment portion of the SON. This will outline the required steps to pay the SONs for the functions performed by them on the Peerplays blockchain. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## **3. Background**

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of SONs. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every SON will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a SONs changes, the transaction signing key of the outgoing SON needs to be removed from the multisig bitcoin wallet and the key of the incoming SON needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

## **4. Process Overview**

Described here is the process to pay the SONs on the Peerplays Blockchain.

## **5. Flow Diagram**

N/A

## **6. Context**

The SON operators will be paid in PPY from the payment pool set up specifically for payments to SONs. This pool will be replenished by depositing a percentage of all the transaction on the Peerplays network. This percentage should be a chain parameter so that it can be changed. The fee will be distributed at the Maintenance intervals based on transactions signed and weight of voting for each SON. Recommended SON payment is 200 PPY daily \(this limit is controlled by \`SONS\_DAILY\_MAX\_REWARD\`\).

## **7. Requirements**

**7.1 Supported Transaction Types**

All operations performed by SONs must be tallied and paid according to budget and defined payout procedure \(as described in sections below\).

System must include a library \(global parameters\) of configurable fee amounts fee amounts associated with each operation. Library must track each operation, its type, fee amount and currency in which fee is charged \(PPY by default\). Fees are set and amendable by committee members in accordance with committee member procedures to update global parameters.

Operations commonly performed by SONs are as follows:

* asset\_issue\_operation
* asset\_reserve\_operation
* proposal\_create\_operation
* proposal\_update\_operation
* proposal\_delete\_operation
* son\_create\_operation
* son\_update\_operation
* son\_delete\_operation
* son\_heartbeat\_operation
* son\_report\_down\_operation
* son\_maintenance\_operation
* son\_wallet\_recreate\_operation
* son\_wallet\_update\_operation
* son\_wallet\_deposit\_create\_operation
* son\_wallet\_deposit\_process\_operation
* son\_wallet\_withdraw\_create\_operation
* son\_wallet\_withdraw\_process\_operation

See Risks section for considerations regarding fee amounts and their implications on financial viability of SON.

**7.2 Collecting Fees**

All operations require a fee to be collected and paid to the network

Fee collection is determined by transaction type:

1. Core asset transaction fees are deducted from payer's account
2. UIA \(pBTC\) transaction fees are converted to core asset using base exchange rate \(note: bBTC to BTC is 1:1\)fee mus

In a scenario where user has 0 PPY, fee must be collected from **fee\_pool**.

**7.3 Determining payout**

Payout must be determined by the number of transactions verified by a node. Higher availability nodes participate in more transactions and therefore receive higher payout.

SONs are paid based on % ratio between total\_transactons\_per\_day and SONS\_DAILY\_MAX\_REWARD, where SON's % share of daily transactions determines what % of SONS\_DAILY\_MAX\_REWARD is paid.

Payout happens only during SON maintenance interval, therefore payout is configurable based on the payment interval and can be configured to happen once every x maintenance intervals.

**7.4 Payment Pool**

Payments to SONs are stored inside son\_budget which functions similarly to witness\_budget. Specifically, son\_budget accumulates transaction fees collected by peerplays network. As described above, payout happens during the maintenance interval .

SONS\_DAILY\_MAX\_REWARD must initially be set to 200 PPY. We may to have to change this as per market realities etc. Currently BTC is the only supported cryptocurrency.

