# son-switchover

## 1. Purpose

The purpose of this document is to outline various scenarios in which SONs will be switched from the top 2/3rd and also among the top 15 minimum ACTIVE SONs.

1. Scope

The functional requirement listed in this document will be limited to the multi-sig key change scenarios only.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of SONs. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every SON will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a SONs changes, the transaction signing key of the outgoing SON needs to be removed from the multisig bitcoin wallet and the key of the incoming SON needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

## 4. Process Overview

Various scenarios are outlined in the Scenarios section

## 5. Context

The SONs has a ranking & grouping. The ranking is depending on the total effective stake of the votes received. We can have infinitely many SONs and rankings. The top 15 nodes aka ACTIVE nodes are the ones which are mandated by the SON implementation. ie at any point of time, there has to be 15 nodes for the SON feature to be operational. Apart from the ACTIVE group of top 15, we also have a 2/3rd top ranked SONs interms of votes which signs the transactions.

Any time there is a change to active SON list, a change to primary wallet scripts will be triggered. Therefore any change to active SONs list must result in creation of new primary wallet using public keys of SONs that remain active after the change. The reason for this is that primary wallet is created based on public keys of all active sons, when the composition of active SON list changes, it affects what keys are associated with the primary wallet.

At each maintenance intervals, list of candidates for active SONs is compared to list of current active SONs, if lists are different, list of current active SONs is understood to change and thus a new primary wallet must be generated.

## 6. Scenarios

Scenarios below are examples of changes that may impact active SONs list and thus trigger re-creation of primary wallet using public keys of SONs on active SONs list.

### **6.1 Ranking change within the top 2/3rd SONs** <a id="FunctionalSpecification-SONsswitchoverscenarios-6.1Rankingchangewithinthetop2/3rdSONs"></a>

In this scenario the SONs will change their ranking and order but the top 2/3rd SONs will remain the same. This means, if there are 15 SONs and the 2/3rd is 10 SONs, the rank of the 10 SONs will be changed. But it will not have any impact on the multi-sig wallet. On using the `list_sons` will provide the new order.

During design, if possible its recommended to have invoke the immediate ranking change only in the case of a change involving the multi-signature key change. ie, this particular scenario can be ignored until the first deposit transaction happens. This will induce a slight delay in the display of ranking of the SONs for the reporting purposes.

### **6.2 Ranking Change due new votes resulting in the change of multi-wallet key** <a id="FunctionalSpecification-SONsswitchoverscenarios-6.2RankingChangeduenewvotesresultinginthechangeofmulti-walletkey"></a>

This scenario addresses the normal case where one or more of the top 2/3rd SONs loses votes and they moves out of the top 2/3rd.

An example scenario is with 15 ACTIVE SONs where the top 10 are forming the 2/3rd quorum. When one of them loses votes, an ACTIVE node or a node outside of TOP 15 ACTIVE nodes replaces its position.

This scenario would trigger:

* creation of a new Bitcoin address
* move the Bitcoin \(BTC\) balances to the new address which is controlled by the new set of top SONs
* The deposit and transfer APIs of the SON should return new multi-signature wallet address

### **6.3 Ranking change - ACTIVE SON change** <a id="FunctionalSpecification-SONsswitchoverscenarios-6.3Rankingchange-ACTIVESONchange"></a>

If an ACTIVE SON receives or loses votes, its effective stake can change. This can trigger the ranking change. The change will be executed at the next `son_maintanance_`interval\` , ie 24 hours or during the next deposit/withdraw operation whichever comes first.

Ranking changes in the list of SONs must result in creation of new multi-signature wallet using public keys of new SONs. Balances are then transferred to the new address.

Note that any changes to list of SONs must result in new wallet being generated and balances transferred to new wallet address.

**6.4 Ranking Change - Disabling an ACTIVE SON**

If an ACTIVE SON disables its node, another node has to replace it.

This can trigger the ranking change. The change will be executed at the next `son_maintanance_`interval\` , ie 24 hours or during the next deposit/withdraw operation whichever comes first.

Ranking changes in the list of SONs must result in creation of new multi-signature wallet using public keys of new SONs. Balances are then transferred to the new address.

### **6.5 Ranking change - an ACTIVE SON goes offline** <a id="FunctionalSpecification-SONsswitchoverscenarios-6.5Rankingchange-anACTIVESONgoesoffline"></a>

In this scenario, an ACTIVE SON but not in the top 2/3rd in terms of cumulative votes received goes down.

* SON goes down/offline
* The SON network records this
* Upon completion of 12 hours after the SON going down/offline event is recorded, the ranking change is triggered
* The Ranking change will be executed on the first deposit transaction or once in 24 hours whichever comes first.
* This approach is done at the cost of status change at the front-end, but to avoid un-necessary operations in the chain

Ranking changes in the list of SONs must result in creation of new multi-signature wallet using public keys of new SONs. Balances are then transferred to the new address.

**6.6. A top 2/3rd SON goes offline**

A high stake SON or a group of SONs goes offline without notice. This scenario or slowness is often observed during large scale internet routing attacks involving BGP poisoning or a data center maintenance where a group of servers becomes unreachable. \(This often happens when certain German data center performs an upgrade or maintenance\).

* If we have ACTIVE SONs, and if need to perform a key change before the on the maintenance interval or before the next deposit transaction is requested, we can proceed with the change of keys
* Upon such a change the funds has to be transferred to the new wallet created
* The fund transfer will be possible if the remaining SONs who originally signed the transaction has enough authority \(weight\) to perform the withdraw operation on the old wallet
* If the remaining SONs doesn't have enough control / weight to operate the multi-signature wallet, the funds will locked in the previous wallet until the necessary SONs becomes active. There seems to no way to address this. But, new deposits to the address will not be entrained as the deposit API will return new Bitcoin address.

If change of keys occurs for any SONs, this event must must result in creation of new multi-signature wallet using new SON keys. Balances are then transferred to the new address.

### **6.7 A top SON goes into maintenance - with key change** <a id="FunctionalSpecification-SONsswitchoverscenarios-6.7AtopSONgoesintomaintenance-withkeychange"></a>

If a SON part of the multi-signature wallet creation with a large stake enough to trigger change in the keys, the maintenance mode should trigger a change in the ranking of the SONs & also a change in the keys.

To avoid frequent key changes, the key change can be handled upon the first deposit or withdrawal. The deposit and withdrawal operations to the SON should be made via calling respective APIs to make this possible.

**IMPORTANT:** The time taken for the wallet change operation should be captured and we need to take this account for fine tuning the strategy for key changes. This is something that we can't easily predict right now, but needs to test and verify.

This scenario must trigger creation of new wallet using SON public keys, and transfer of balance to new wallet address due to change in SON composition and public keys.

