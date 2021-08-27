# son-configuration

## Purpose

The purpose of this document is to provide the design for the SON configuration.

## Scope

This document describes the SON configuration based on the functional specifications [Functional Specification - SON Configuration](file:///C:/wiki/spaces/PIX/pages/288817497/Functional+Specification+-+SON+Configuration) excluding the functionality that has been described in the Low Level Design document [Wallet Commands for SON](file:///C:/wiki/spaces/PIX/pages/333545473/Wallet+Commands+for+SON).

## Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of SONs. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every SON will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a SONs changes, the transaction signing key of the outgoing SON needs to be removed from the multisig bitcoin wallet and the key of the incoming SON needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

## Process Overview

Described here is the design for configuring the SONs on a node

## Context

SONs have to be implemented as a plugin i.e. it shouldn't be mandatory for a node operator to register as an SON. It should be an option that can be chosen at the time of starting/restarting a node. Moreover, there shouldn't be an issue when replaying the chain even if the plugin is disabled. Also, the number of active SONs should be a chain parameter and the witnesses should be able to vote and change it.

## son\_plugin

son\_plugin class should inherit from the graphene::app::plugin class and the methods plugin\_set\_program\_options\(\), plugin\_initialize\(\), plugin\_startup\(\) and plugin\_shutdown\(\) can be overridden to initialize the rpc client and to start/stop the bitcoin zmq listener.

## son\_api

All the APIs related to sidechain should be moved to the son\_api class in the graphene::son namespace together with the son\_plugin so that it is easy to maintain the project.

## Register son\_plugin

The SON plugin has to be registered in programs&gt;witness\_node&gt;main.cpp using the register\_plugin\(\) method of node object if the options contain the string "son-enable" like below:

if\(options.count\("son-enable"\)\)

node-&gt;register\_plugin&lt;son::son\_plugin&gt;\(\)

This has to be done before calling the initialize\_plugins\(\) method of the node object.

