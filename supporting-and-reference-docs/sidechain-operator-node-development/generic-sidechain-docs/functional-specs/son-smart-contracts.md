# SON Smart Contracts

## Purpose

The purpose of this document is to outline the steps required to use smart contracts for Inter-blockchain communication with blockchains that support smart contracts such as Ethereum, EOS and the likes.

## Scope

The functional requirement listed in this document will be limited to the use of smart contracts to enable inter-blockchain communication between Peerplays and other blockchains that support smart contracts. This will outline the required steps to create smart contracts on other blockchains which can then be called by the Peerplays blockchain to perform various operations such as withdrawal of the cryptocurrency back to the user. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses. As per the current implementation of Sidechain, a multisig bitcoin wallet will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every Peerplays witnesses will have a bitcoin transaction signing key for this multisig bitcoin wallet and will be required to sign any withdrawal transaction. When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multisig bitcoin wallet and the key of the incoming witness needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\). The SONs will be independent of the witnesses and don't need to be changed much often.

## Process Overview

Described here is the process to use smart contracts on enable inter-blockchain communication between Peerplays and other blockchains that support smart contracts. For ease of understanding, we will use EOS as an example for the parent chain.

## Context

To facilitate the transfer of tokens from other blockchains to Peerplays, smart contracts have to be deployed on both Peerplays and the EOS blockchain. We would also need to check the smart contract tables every 5-10 seconds, scanning for transactions to relay across the network. This can be done by polling functions relay\_eos and relay\_ppy running on the SONs. relay\_eos will check the smart contract tables on the EOS and relay\_ppy will check the smart contract tables on PPY. These functions may be called IBC Relays. 

**This cannot be implemented until we have Smart Contracts implemented in Peerplays.**

## Smart Contracts Flow

1. The user will initiate an inter-blockchain transaction orig\_trx on the EOS mainnet using peerplays.io contract. The transaction information will be recorded in the origtrx table of the peerplays.io contract on the EOS mainnet side;
2. relay\_eos will get the transaction and transaction-related information \(block information and Merkle path\), then pass it to the relay of Peerplays side \(relay\_ppy\);
3. relay\_ppy constructs 'cash' transaction and calls the **cash** action of the eos.io contract on the Peerplays side. If the call succeeds in the **cash** function, the corresponding token will be issued to the target user after deducting the fees;
4. relay\_ppy will get cash\_trx and related information \(block information and Merkle path\) and pass them to relay\_eos. relay\_eos will construct the **cashconfirm** transaction and call the **cashconfirm** action of the peerplays.io contract on the EOS mainnet side, which will delete the original transaction record in peerplays.io contract. This will complete the inter-blockchain transaction.
5. At step 3, if the call fails, relay\_ppy will get an exception in the cash\_trx and pass them to relay\_eos. relay\_eos will construct a **cashrefund** transaction and call the **cashrefund** action of the peerplays.io contract on the EOS mainnet side, which will refund the amount to the sender after deducting the fees.

## References

[https://github.com/boscore/Documentation/blob/master/IBC/EOSIO\_IBC\_Priciple\_and\_Design.md](https://github.com/boscore/Documentation/blob/master/IBC/EOSIO_IBC_Priciple_and_Design.md)

