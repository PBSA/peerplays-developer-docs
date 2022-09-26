# generic-sidechain-deposit-hld

## Objective

This document is intended to outline generic design of deposit handling by Peerplays sidechain operating node. This interface will be extended to meet various other integrations.

Sidechain operating node will monitor target sidechain for events of interest, namely transfer of assets to a Peerplays address. When transfer happens, sidechain will process it, in order to pick up information needed to handle it. Handling deposits will create certain amount of tokens on a Peerplays network. This amount will match the amount of assets on target sidechain by predefined exchange rate.

## Assumptions

* Peerplays SON network is able to control (transfer or create) assets on Peerplays network, in order to send assets to a user account. E.g. Peerplays SONs network has control over an account on Peerplays network which holds initial ballance of Peerplays tokens, or is able to mine them to user account, or create them out of thin air and transfer them to user during deposit handling.
* Peerplays SON network have full control over address/account on every supported sidechain. This address/account may be single signature wallet, multi-signature wallet, script, smart contract, or any other entity supported by a target sidechain, which is able to receive assets from users. This is the address where user should send his assets to, in order to get Peerplays tokens on Peerplays network.
* Sidechain operating node has access to assets exchange rate list, containing exchange rates for every asset on every supported sidechain network. This list should be updateable by Peerplays network administrators.
* Sidechain operating node has access to user accounts mapping list, containing user addresses on supported sidechains. Key value in this mapping is user’s Peerplays account. E.g. \[PeerplaysAccount, BitcoinAccount, EthereumAccount, …]. All sidechain accounts should be created by user (e.g. user can enter them during registration as a Peerplays user). Account creation on target sidechain should not be handled by Peerplays, because of increased risk of handling their private keys. This list should be updateable by user.

## Block diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/14HFGLqD8IV3ics1ojFcZ2xC6hRtFXLck/view?usp=sharing](https://drive.google.com/file/d/14HFGLqD8IV3ics1ojFcZ2xC6hRtFXLck/view?usp=sharing)

## Description

* As described in [Generic Sidechain High Level Design](generic-sidechain-high-level-design.md) , when event of interest, transfer to a certain address on a target sidechain occurs, a sidechain handler will process it, and save the event data into Peerplays network for further processing.
* Sidechain handler will identify user depositing funds, by searching transaction’s source address in user accounts mapping list, in order to find which Peerplays network address matches sidechain transaction source address. Peerplays network will transfer assets to this address (target address).
* Sidechain handler will get the exchange rate from Exchange rate list, in order to calculate exact amount that needs to be transfered to the target address.
* SONs will synchronize between them selves, by exchanging information about new transaction on sidechain and information about transfer that needs to be executed on Peerplays network. Once all active SONs have same information about transaction, randomly selected SON will create transaction and start singing process. Source address of this transfer will be SONs controlled account for issuing assets to users.
* When first signature is added to transaction, signed transaction will be sent through SONs network, so that other SONs can sign it. At least 8 SONs needs to sign this transaction. We can also go with SONs with at least 2/3 of total weight signature, if that's supported by Bitshare. For now, it does not look like it is. Last SON signing transaction will broadcast it to the network. Tutorial on how to use multisig in Bitshares is here: [https://github.com/bitshares/bitshares1-core/wiki/multisig](https://github.com/bitshares/bitshares1-core/wiki/multisig)
* All sidechain specific functionalities will be implemented in sidechain handler.
* All general functionalities will be implemented in sidechain handler base class.

## Sequence diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing](https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing)

Same sequence diagram applies for all sidechain handlers. It shows interactions between components in a single sidechain handler.
