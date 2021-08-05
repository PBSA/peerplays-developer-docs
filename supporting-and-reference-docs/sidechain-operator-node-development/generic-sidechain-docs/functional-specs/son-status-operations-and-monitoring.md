# SON Status Operations & Monitoring

## 1. Purpose

The purpose of this document is to outline requirements for status monitoring of Sidechain Operation Nodes for purpose of determining whether SONs are active and able to participate in blockchain transactions.

## 2. Scope

The functional requirement listed in this document will be limited to the configuration portion of the SON. This will outline the required steps that will be performed by the Peerplays node operator to enable/disable Sidechain plugin in the Peerplays blockchain from the command line. Also outlined here will be the sequence in which the required steps will be performed including:

* SON heartbeats
* SON maintenance mode actions
* SON de-registration

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of SONs. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every Peerplays witnesses will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a SONs changes, the transaction signing key of the outgoing witness needs to be removed from the multisig bitcoin wallet and the key of the incoming witness needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\). The SONs will be independent of the witnesses and don't need to be changed much often.

## 4. Process Overview

Described here is the process of SON status reporting \(heartbeat\) and maintenance mode activities.

1. System sends heartbeat to registered SONs based on specified frequency
2. Process responses:
   1. Active and Request Maintenance SONs will respond to heartbeat
      1. Update statistics with last active timestamp and transactions signed updates
   2. In maintenance and inactive SONs will not respond to heartbeat
      1. inactive flow
      2. in maintenance flow

## 5. Context

The Peerplays blockchain will allow the node operators to enable/disable the Sidechain functionality on their node. They will have to stake PPY 50 to be able to register as an SON. As soon as the Sidechain functionality is enabled/disabled and the user vests 50 PPY, the Peerplays blockchain will add the node to its list of Sidechain enabled nodes and make the node operator available for SON voting. An SON enabled node will be able to operate as an SON only when it receives the votes required to become one of the top 15 SONs.

## 6. Flow Diagram

\[TBD\]

## **7. Requirements**

SON monitoring is required because only active SONs can participate in blockchain transactions.

### **7.1. SON Status monitoring and statistics \(Heartbeat\)**

System must include an automated heartbeat check that monitors status of registered SONs per each 180 second interval \(heartbeat interval\). This interval must be configurable via **chain\_parameters** in **extensions.son\_heartbeat\_frequency**, and set to 180 seconds by default. \(Other configuration may be possible via genesis.json\). Note that this interval may be different in production, but for testing purposes it should be within 3 minutes.

Heartbeats must be sent by SONs who are in **active**, or **request\_maintenance** status. All sent heartbeat activity must be logged. SONs in inactive status must not send heartbeat.

The following statistics must be tracked and logged as part of heartbeat monitoring:

* number of transactions signed - number of transactions signed by SON. This value must be updated each time a transaction is signed. This counter is reset during SON rewards.
* total downtime - compounded time that SON was down represented in HH:MM:SS format. This value must be updated to compounds all current interval downtime values.
* current interval downtime - Time since last transition to in\_maintenance. This counter is reset during maintenance
* last down timestamp - timestamp of last transition to in\_maintenance status
* last active timestamp - timestamp of last transition to active status, or last heartbeat where status is active or request\_maintenance.

SONs in active or request\_maintenance must be treated as active for the purpose of transaction signing.

SONs in in\_maintenance or inactive status must not participate in transaction signing.

Statistics for active SONs must be updated after every heartbeat where SON is active, request maintenance.

### **7.2. Requesting maintenance**

System must include a wallet command which requests SON to be placed in maintenance mode. Requesting SON maintenance must be available to SONs in active status. Once request SON maintenance is initiated, target SON must be set to **in\_maintenance** at the upcoming chain maintenance interval.

\[TBD/INACCURATE. TO CONFIRM WITH SATYA  
SONs in maintenance modes can be changed to active or inactive mode via voting process described in SON Voting and Consensus requirements. Voting and change of status must be initiated by sending a heartbeat.\]

System must allow cancelling of maintenance request via **cancel\_request\_son\_maintenance** command. System must restrict this command to SONs in request\_maintenance status.

After successful execution of cancel maintenance request, SON must be placed into active status.7.3

### **7.3. Reporting unavailable SONs**

### **7.4 SON De-registration Operation**

System must de-register SONs that have been in maintenance state for longer than allowed. Allowed maintenance threshold must be a configurable parameter extensions.son\_deregister\_time, with 12 hours being the default value. Alternatively, deregister threshold may be specified in genesis.json

Once system identifies an SON that has been in maintenance state for longer than specified threshold, system must create a proposal with son delete operation raised by one of schedules SONs.

### **Declaring inactive**

SON monitoring process must track SONs which miss a heartbeat and declare an SON inactive if an SON misses two consecutive heartbeats. Tracking missed heartbeats must reset for each SON where a heartbeat is received after a missed heartbeat.

