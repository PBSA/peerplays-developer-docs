# bitcoin-deposit-handling-lld

### Objective

This document is intended to outline generic design of a Bitcoin deposit handler, a component used for monitoring and processing deposits from a Bitcoin network.

For general information on deposit handlers, read:\
[Generic Sidechain Deposit HLD](https://peerplays.atlassian.net/wiki/spaces/PIX/pages/351961138/Generic+Sidechain+Deposit+HLD)

### Asumptions

* Peerplays SON network is able to control (transfer or create) assets on Peerplays network - FULFILLED
* Peerplays SON network have full control over address/account on Bitcoin sidechain - FULFILLED (multisig addresses)
* Sidechain operating node has access to assets exchange rate list, containing exchange rates for every asset on every supported sidechain network - FULFILLED For Bitcoin, exchange rate is fixed 1 BTC = 1 PPY
* Sidechain operating node has access to user accounts mapping list, containing user addresses on supported sidechains - FULLFILLED

## Block diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/14HFGLqD8IV3ics1ojFcZ2xC6hRtFXLck/view?usp=sharing](https://drive.google.com/file/d/14HFGLqD8IV3ics1ojFcZ2xC6hRtFXLck/view?usp=sharing)

## Description

* Deposit process is initiated by sending BTC to a Bitcoin address which is registered as a sidechain user address for deposits
* Bitcoin listener will pick up this transaction
* Bitcoin sidechain handler will create **sidechain\_event\_data** data structure, with the information about transaction, and pass it to the **sidechain\_event\_data\_received**
* **sidechain\_event\_data\_received** will create a proposal for creating deposit descriptor object **son\_wallet\_deposit\_object**
* When proposal is approved, object will be created, or if it is already created by another SON, it will be confirmed
* Scheduled SON will start processing withdrawals by creating Bitcoin transaction for sending funds from Bitcoin address which is registered as a sidechain user address for deposits to primary wallet (Bitcoin multisig address controlled by active SONs)
* Bitcoin transaction will be signed and sent to Bitcoin node
* User will receive Peerplays core asset matching the amount of deposited BTC
* Scheduled SON will mark son\_wallet\_deposit\_object as processed
