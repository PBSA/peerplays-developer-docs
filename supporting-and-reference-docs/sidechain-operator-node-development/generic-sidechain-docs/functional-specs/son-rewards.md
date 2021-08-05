# SON rewards

## **1. Purpose**

The purpose of this document is to outline the steps required to pay the SONs for their contribution to the Peerplays network.

## **2. Scope**

The functional requirement listed in this document will be limited to the payment portion of the SON. This will outline the required steps to pay the SONs for the functions performed by them on the Peerplays blockchain. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## **3. Background**

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of SONs. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every Peerplays witnesses will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a SONs changes, the transaction signing key of the outgoing witness needs to be removed from the multisig bitcoin wallet and the key of the incoming witness needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\). The SONs will be independent of the witnesses and don't need to be changed much often.

## **4. Process Overview**

Described here is the process to pay the SONs on the Peerplays Blockchain.

## **5. Flow Diagram**

N/A

## **6. Context**

The SON operators will be paid in PPY from the payment pool set up specifically for payments to SONs. This pool will be replenished by depositing a percentage of all the transaction on the Peerplays network. This percentage should be a chain parameter so that it can be changed. The fee will be distributed at the Maintenance intervals based on transactions signed and weight of voting \(therefore voting power affects payout\) for each SON. Currently set maximum SON payment is 200 PPY daily and is specified in \`SONS\_DAILY\_MAX\_REWARD\`.

## **7. Requirements**

**7.1 Supported Operation Types**

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

**7.2 Paying Witnesses**

Whenever a witness generates a new block, reward amount must be immediately deposited into witness account's vesting balance.

The amount must be withdrawn from witness\_budget

**7.3 Collecting Fees**

All operations require a fee to be collected and paid to the network. 

Fee collection is determined by operation type \(as described in section Supported Operation Types\):

1. Core asset transaction fees are deducted from payer's account
2. UIA \(pBTC\) transaction fees are converted to core asset using base exchange rate \(note: bBTC to BTC is 1:1\)fee mus

In a scenario where user has 0 PPY, fee must be collected from **fee\_pool**.

**7.4 Calculating budget**

The equation for calculating SON budget is as follows:

**current\_supply = current\_supply + \(required\_witness\_budget + required\_worker\_budget + required\_son\_budget - leftover\_worker\_funds - core\_accumulated\_fee - leftover\_witness\_budget - leftover\_son\_budget\)**

**reserve\_supply = max\_supply - current\_supply**

* required\_witness\_budget - required witness budget till next maintenance interval 
* required\_worker\_budget - required worker budget till next maintenance interval 
* required\_son\_budget - reserved SON budget to be paid out in the next maintenance interval = \(chain\_parameters.son\_pay\_max\)
* leftover\_worker\_funds - Remaining funds from previous worker payout
* core\_accumulated\_fee - Accumulated fee \(after all the cuts removed from it\),  since the last maintenance interval.
* leftover\_witness\_budget -  Leftover funds from previous witness budget \( happens if any block is missed in between \)
* leftover\_son\_budget - Leftover funds from previous son budget \( happens if there are no SON transactions on the network \)

other required variables:

* total\_transactions\_per\_day - tracks number of transactions SON participated in
* SONs\_REWARD\_POOL - stores SON reward amounts
* SONS\_DAILY\_MAX\_REWARD - configurable variable which sets maximum size of reward
* son\_pay\_max - dictates how much SONs are to be paid out it next payment interval

**7.5 Determining payout**

Payout must be determined by the number of transactions verified by a node. Higher availability nodes participate in more transactions and therefore receive higher payout.

SONs are paid based on % ratio between total\_transactions\_per\_day and SONS\_DAILY\_MAX\_REWARD, where SON's % share of daily transactions determines what % of SONS\_DAILY\_MAX\_REWARD is paid. Payout happens only during SON maintenance interval, therefore payout is configurable based on the payment interval and can be configured to happen once every x maintenance intervals.

**7.6 Payment Pool**

Payments to SONs are stored inside son\_budget which functions similarly to witness\_budget. Specifically, son\_budget accumulates transaction fees collected by peerplays network. As described above, payout happens during the maintenance interval .

SONS\_DAILY\_MAX\_REWARD must initially be set to 200 PPY. We may to have to change this as per market realities etc. Currently BTC is the only supported cryptocurrency.

Note: SONS\_DAILY\_MAX\_REWARD must be reviewed in production to determine whether this limit is too low or restrictive. Depending on the fees associated with each transaction \(as described in Transaction Type section above\), highly active SONs may perform a number of operations and reach daily max limit quickly thus creation a disproportion between operations performed vs max daily reward.

## **8 Risks**

SON concept has proven to be technically feasible, however, there are financial viability implications that must be taken into account.

SON operations must be priced in ways that offset electricity cost of node operators by a margin that makes it worthwhile to operate a node, however also low enough to make the blockchain cost effective to its users.

Inability to find a fee model attractive to node operators will impede availability of SONs \(for which there is a minimum threshold\), while high fees are likely to impede user interest due to high cost.

Note that because fees may be amended by committee members, fees they set may create risks mentioned above. It is recommended that committee member operations in relation to fees are closely monitored.

##  Sample test: 

1. configure **witness\_pay\_per\_block** chain parameter = 1
2. configure **son\_pay\_daily\_max** chain extension parameter = 10000 \( As we don’t have enough fee collected on our chain, keeping it low is essential to test properly\)
3. configure **son\_pay\_time** chain extension parameter = **maintenance\_interval** or multiples of **maintenance\_interval**. \(Eg. x, 2x, 3x, 4x etc x = **maintenance\_interval**\). As budget is calculated during maintenance period **son\_pay\_time** should be multiples of **maintenance\_interval**.
4. Temporarily, **txs\_signed** of **son\_statistics\_object** is incremented through son heartbeats to enable testing of SON Rewards. \( **txs\_signed** can be obtained by **get\_object 2.24.0** command\). **txs\_signed** are reset and added to **total\_txs\_signed** if SONs are paid out successfully.
5. Check that the **get\_dynamic\_global\_properties** has **son\_budget** and **last\_son\_payout\_time** updated correctly like below,

   get\_dynamic\_global\_properties  
   {  
   "id": "2.1.0",  
   "random": "0f0b35fba1f53d22384e642356c25510e63f18b7",  
   "head\_block\_number": 4708,  
   "head\_block\_id": "00001264be90f462d553e2edee2e7fee698215c1",  
   "time": "2020-03-19T10:25:42",  
   "current\_witness": "1.6.2",  
   "next\_maintenance\_time": "2020-03-19T10:30:00",  
   "last\_budget\_time": "2020-03-19T10:24:00",  
   "witness\_budget": 93,  
   "**last\_son\_payout\_time**": "2020-03-19T10:24:00",  
   "**son\_budget**": 10000,  
   "accounts\_registered\_this\_interval": 0,  
   "recently\_missed\_count": 7,  
   "current\_aslot": 8917831,  
   "recent\_slots\_filled": "1298074214633706907132624082304903",  
   "dynamic\_flags": 0,  
   "last\_irreversible\_block\_num": 4700  
   }

6. **list\_account\_balances** &lt;sonaccount01&gt; to check that the pay is added to the SON account’s balance.
7. If there are 13 active SONs, and each SON sent 3 heartbeats before next payout time, You can find below snippet in the log during the chain maintenance, pay 769 to 2.24.0 for 3 transactions signed total txs = 13 x 3 = 39. so pay for each SON -&gt; \(3/39\)x10000 = 769

