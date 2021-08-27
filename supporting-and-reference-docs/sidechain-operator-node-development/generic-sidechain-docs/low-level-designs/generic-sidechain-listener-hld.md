# generic-sidechain-listener-hld

## Objective

This document is intended to outline generic design of a blockchain listener, a component used for monitoring changes on a target blockchain.

## Assumptions

* Blockchain node provides interface for monitoring changes in a blockchain.
* Monitoring is based on new block, new transaction or filtered single event \(like transfer operation to specific address\).
* There is a sufficient C/C++ library for connecting to monitoring interface.

## Description

Implementing blockchain listener is highly blockchain specific task. There are no standardised interface that all blockchain nodes use for monitoring changes. We need to investigate target blockchain, get familiar with its way of monitoring changes, available protocols, libraries, etc…

The goal of processing data provided by listener is to find event of interest, in particular, a transfer operation to a target address \(e.g. address of Peerplays primary wallet\).

In order to be able to do this, we can put in place some common rules each listener needs to follow, in order to report usable data to event handler.

* Listener needs to provide information about event of interest in a way that will enable event handler to narrow its processing to minimum.
* Event handler, when processing event data provided by listener, must be able to get at least following information:
  * Sidechain transaction unique identifier
  * Transfer operation’s source address
  * Transfer operation’s target address
  * Transfer amount
  * Transaction fee \(for transfer operation\)
* From what we know so far, listener can report new block, a transaction, or filtered single event \(like specific operation involving specific address\).
  * If new block is reported, we can go through block content, and look for events of interest, like specific operation involving specific address
  * If new transaction is reported, we can investigate what kind of operation it contains, and which addresses are involved
  * If listener supports filtered events, we can create a filter in a way that will report only event of interest

## Block Diagram

Link to [draw.io](http://draw.io/) file:

[https://drive.google.com/file/d/1ErmQfeaWa9m67si4hb0RZs54WVYWR0pC/view?usp=sharing](https://drive.google.com/file/d/1ErmQfeaWa9m67si4hb0RZs54WVYWR0pC/view?usp=sharing)

## Sequence diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing](https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing)

Sequence diagram shows sidechain listener interactions with the rest of the sidechain components.

## Suggested implementation

As the implementing blockchain listener is highly blockchain specific task, we will not put any constraints on a listener implementation or require specific interface. Listener should be implemented any way it works, and sidechain handler task is to process input from event, use communication interface, if needed, to get the minimum required information about transfer, and pass it to higher level of processing.

