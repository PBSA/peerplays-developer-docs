# bitcoin-withdrawal-handling-lld

## Bitcoin Withdrawal Handling LLD

This document assumes that the multi-signature wallet is existing

### Objective

This document is intended to outline generic design of a Bitcoin withdrawal handler, a component used for processing withdrawals to Bitcoin network.

For general information on deposit handlers, read:  
[Generic Sidechain Withdrawal HLD](file:///C:/wiki/spaces/PIX/pages/352026683/Generic+Sidechain+Withdrawal+HLD)

### Asumptions

* Peerplays SON network is able to transfer assets on Bitcoin network - FULFILLED
* Peerplays SON network have dedicated account on Bitcoin sidechain - FULFILLED \(multisig addresses\)
* Sidechain operating node has access to assets exchange rate list, containing exchange rates for every asset on every supported sidechain network - FULFILLED For Bitcoin, exchange rate is fixed 1 BTC = 1 PPY
* Sidechain operating node has access to user accounts mapping list, containing user addresses on supported sidechains - FULLFILLED

## Block diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/1qSWvcxfWVEwJV2nZA-xTApTSi5rH\_1uv/view?usp=sharing](https://drive.google.com/file/d/1qSWvcxfWVEwJV2nZA-xTApTSi5rH_1uv/view?usp=sharing)

## Description

* Withdrawal process is initiated by sending Peerplays core asser to a SON shared account
* Peerplays listener will pick up this transaction
* Peerplays sidechain handler will create **sidechain\_event\_data** data structure, with the information about transaction, and pass it to the **sidechain\_event\_data\_received**
* **sidechain\_event\_data\_received** will create a proposal for creating withdrawal descriptor object - **son\_wallet\_withdrawal\_object**
* When proposal is approved, object will be created, or if it is already created by another SON, it will be confirmed
* Scheduled SON will start processing withdrawals by creating Bitcoin transaction for sending funds from primary wallet \(Bitcoin multisig address controlled by active SONs\) to the a Bitcoin address which is registered as a sidechain user address for withdrawals
* Bitcoin transaction will be signed and sent to Bitcoin node
* Scheduled SON will mark son\_wallet\_withdrawal\_object as processed

