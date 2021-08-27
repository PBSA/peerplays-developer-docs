# changeover and SON maintenance scenarios lld

## 1. Purpose

The purpose of this document is to outline the existing changeover process in Sidechain and list the SON maintenance scenarios \(both user-triggered and abruptly downed servers\).

## 2. Scope

The design listed in this document is limited to the listing of the scenarios that can arise due to maintenance and how to tackle the PW changes arising out of the change of SONs.

This document also outlines the current implementation of Sidechain and whats missing from that implementation and how it is different from the current SONs requirement.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses.

As per the current implementation of Sidechain, a multi-sig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users.

Every Peerplays witnesses will have a bitcoin transaction signing key for this multi-sig bitcoin address and will be required to sign any withdrawal transaction.

When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multi-sig bitcoin address and the key of the incoming witness needs to be added.

The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. How it all works

There are two types of bitcoin addresses,

1. PW Address
2. Deposit Address of a User.

Following are the list of steps needed for a user to transfer BTC,

1. A PW Address is created which acts as a global address holding all the user funds. 
2. A user issues create\_bitcoin\_address and gets a bitcoin address multi-signed by SONs \( 10-of-15 multi-sig Proposed currently \). This is called Deposit Address.
3. The user transfers BTC funds to the deposit address.
4. Bitcoin block listener catches the transaction if any vout has any of the deposit addresses.
5. A proposal to send the BTC from the user Deposit Address to PW address is initiated by SONs and if approved, a bitcoin transaction signed by the top 2/3 SONs is sent out.
6. After receiving confirmation from Bitcoin \( 6 blocks confirmation \) a proposal to issue pBTC to the user's account is initiated by SONs.
7. If the proposal is approved, pBTC is issued to the user's peerplays account.
8. External miner fee is also taken into consideration for the bitcoin transaction initiated from deposit address to PW address apart from fees of proposals initiated.

## 5. Current Sidechain Implementation

In the current sidechain implementation, Witnesses act as signatories and follow 5-of-15 multi-sig model.

Each witness has the same weight associated with its signing key.

PW address is changed if more than 5 witnesses are voted out which is very unlikely.

All the user deposit addresses are also needed to be changed along with PW address.

More info can be found about sidechain implementation on this site.

## 6. Challenges for the proposed SONs Implementation

The current proposal of SONs takes DPOS mechanism into consideration for associating weights with signatures of each SON.

Based on the votes tallied each SON will have a different weight of the signature in each bitcoin multi-sig address \(10-of-15 multi-sig\) that's issued on Peerplays network.

### 6.1 Disadvantages with current weighted model

1. With the weighted signing, if any of the SONs can accumulate enough weight which is more than 1/3 \(34%\), we will have a single point of failure model.
2. If such a SON \( which has more than 1/3 or at least 34% weight\) goes down abruptly, there is no other way but to change the PW address.
3. PW multi-sig address might need to be changed multiple times in this model because of the weights.
4. Loss of miners fee associated with the transfer of BTC from old PW address to new PW address.

### 6.2 Advantages with equally weighted SONs model

1. With 10-of-15 multi-sig equally weighted model, it is almost impossible for more than 5 SONs to go down at a time. Thus very less chance of SON network stall.
2. PW multi-sig address need not be changed multiple times which can save us the miner fee that's charged for sending BTC from old PW to latest PW.

## 7. Scenarios Captured for PW address change

The idea is that immediately after the initial hard fork with SON change, a new PW address is created and a buffer amount of bitcoins are sent to it.

This buffer is to offset the mining fee associated with the transfer of BTC from old PW address to the new PW address.

### 7.1 De-Register issued by Active SON user

### 7.1.1 SON weight &lt; \( 1/3 or 33.33% \) – **PW Address remains the same.**

If the active SON weight is less than the 1/3 or 33.33% then there is no need to change the PW address.

After the maintenance interval, a new set of active SONs are active but the PW address remains the same.

==&gt; Transfers from user deposit addresses to PW Address goes on as usual.

### 7.1.2 SON weight &gt; \( 1/3 or 33.33% \) – **PW Address changes.**

We will wait until the maintenance interval, tally votes again during the maintenance interval.

If the active SON weight is greater than the 1/3 or 33.33% then a new PW address is assigned.

Funds from the old PW Address are transferred to the new PW Address.

The SON is removed from the active list.

==&gt; All the transactions for transferring BTC from Deposit address to PW Address are stuck till the SON node comes back.

==&gt; Deposit addresses have to be changed to be signed by the new set of SONs.

### 7.2 De-Register issued by Inactive SON user – **PW Address remains the same.**

If an inactive SON is de-registered, there is no need to change the PW Address.

After the maintenance interval, we can allow the user to claim his vesting balance after 2 days.

==&gt; Transfers from user deposit addresses to PW Address goes on as usual.

### 7.3 Active SON Abruptly Down

### 7.3.1 SON weight &lt; \( 1/3 or 33.33% \) – **PW Address remains the same.**

If the active SON weight is less than the 1/3 or 33.33% then there is no need to change the PW address.

After the maintenance interval, a new set of active SONs are active but the PW address remains the same.

==&gt; Transfers from user deposit addresses to PW Address goes on as usual.

### 7.3.2 SON weight &gt; \( 1/3 or 33.33% \) – **PW Address changes.**

We will wait until the maintenance interval, tally votes again during the maintenance interval.

If the active SON weight is greater than the 1/3 or 33.33% then a new PW address is assigned.

Funds from the old PW Address are transferred to the new PW Address.

The SON is removed from the active list.

All the transactions for transferring BTC from Deposit address to PW Address are stuck till the SON node comes back.

==&gt; All the transactions for transferring BTC from Deposit address to PW Address are stuck till the SON node comes back.

==&gt; Deposit addresses have to be changed to be signed by the new set of SONs.

### 7.4 Inactive SON\(s\) Abruptly Down – **PW Address remains the same.**

If an inactive SON is down, there is no need to change the PW Address.

After the maintenance interval, we can allow the user to claim his vesting balance after 2 days.

==&gt; Transfers from user deposit addresses to PW Address goes on as usual.

### 7.5 Multiple Active SONs Abruptly Down

### 7.5.1 Sum of the downed SONs Weight &lt; \( 1/3 or 33.33% \) – **PW Address remains the same.**

If the weight of SONs went down is less than the 1/3 or 33.33% then there is no need to change the PW address.

After the maintenance interval, a new set of active SONs are active but the PW address remains the same.

==&gt; Transfers from user deposit addresses to PW Address goes on as usual.

### 7.5.2 Sum of the downed SONs weight &gt; \( 1/3 or 33.33% \) – **PW Address changes.**

We will wait until the maintenance interval, tally votes again during the maintenance interval.

If the sum of the weight of the active SONs that are down is greater than the 1/3 or 33.33% then a new PW address is assigned.

Funds from the old PW Address are transferred to the new PW Address.

The SONs are removed from the active list.

All the transactions for transferring BTC from Deposit address to PW Address are stuck till one or more SON nodes comes back which allow us to hit the required threshold weight of 2/3 or 66.67%.

==&gt; All the transactions for transferring BTC from Deposit address to PW Address are stuck till one or more SON nodes comes back which allow us to hit the required threshold weight of 2/3 or 66.67%.

==&gt; Deposit addresses have to be changed to be signed by the new set of SONs.

### 7.6 Active SON \( weight &gt; 1/3 or 33.33% \) goes down before Bitcoin transaction is confirmed – **PW Address changes.**

If an active SON with enough weight to stall the SON network goes down in the middle of transfer of BTC from user's deposit address to PW address and then the transaction is not confirmed on Bitcoin network, then the funds are stuck in the user deposit address till the SON comes back.

During maintenance, a new set of SONs are elected.

PW Address is changed with new signatories.

