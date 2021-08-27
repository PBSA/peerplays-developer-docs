# generic-sidechain-withdrawal-hld

## Objective

This document is intended to outline generic design of withdrawal handling by Peerplays sidechain operating node. This interface will be extended to meet various other integrations.

Sidechain operating node will monitor Peerplays network for events of interest, namely transfer of Peerplays tokens to a shared SON account. When transfer happens, sidechain will process it, in order to pick up information needed to handle it. Handling withdrawals will create sidechain transaction, transferring certain amount of sidechain currency to the user who made the payment to the hared SON account. This amount will match the amount of Peerplays tokens by predefined exchange rate.

## Assumptions

* Peerplays SON network is able to transfer assets on sidechain networks, in order to send assets to a user account. E.g. Peerplays SONs network has control over an multi-signature account on Bitcoin, Ethereum, EOS network which holds ballance of sidechain currency and is able to transfer them to the sidechain user accounts.
* Peerplays SON network have dedicated account on every supported sidechain. This address/account is a multi-signature account or script or smart contract, or any other entity supported by a target sidechain, which is able to send assets to users. This is the address where SON network will keep its sidechain asset ballance.
* Sidechain operating node has access to assets exchange rate list, containing exchange rates for every asset on every supported sidechain network. This list should be updateable by Peerplays network administrators.
* Sidechain operating node has access to user accounts mapping list, containing user addresses on supported sidechains. Key value in this mapping is user’s Peerplays account. E.g. \[PeerplaysAccount, BitcoinAccount, EthereumAccount, …\]. All sidechain accounts should be created by user \(e.g. user can enter them during registration as a Peerplays user\). Account creation on target sidechain should not be handled by Peerplays, because of increased risk of handling their private keys. This list should be updateable by user.

## Block diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/1qSWvcxfWVEwJV2nZA-xTApTSi5rH\_1uv/view?usp=sharing](https://drive.google.com/file/d/1qSWvcxfWVEwJV2nZA-xTApTSi5rH_1uv/view?usp=sharing)

## Description

* The withdrawal is initiated by user, by transfering Peerplays core asset to SON shared account.
* As described in [Generic Sidechain High Level Design](https://peerplays.atlassian.net/wiki/spaces/PIX/pages/352026689/Generic+Sidechain+High+Level+Design) , when event of interest, transfer to a certain address on a target sidechain occurs, a sidechain handler will process it, and save the event data into Peerplays network for further processing.
* Sidechain handler will identify user depositing funds, by searching transaction’s source address in user accounts mapping list, in order to find which Peerplays network address matches sidechain transaction source address. Peerplays network will transfer assets to this address \(target address\).
* Sidechain handler will get the exchange rate from Exchange rate list, in order to calculate exact amount that needs to be transfered to the target address.
* SONs will synchronize between them selves, by exchanging information about new transaction on Peerplays network and information about transfer that needs to be executed on sidechain network. Once all active SONs have same information about transaction, randomly selected SON will create transaction and start singing process. Source address of this transfer will be SONs controlled account for transferring sidechain assets to users.
* When first signature is added to transaction, signed transaction will be sent through SONs network, so that other SONs can sign it. Depending on a sidechain features, at least 8 SONs needs to sign this transaction. We can also go with SONs with at least 2/3 of total weight signature, if that's supported by the sidechain. Last SON signing transaction will broadcast it to the network. Tutorial on how to use multisig in Bitshares is here: [https://github.com/bitshares/bitshares1-core/wiki/multisig](https://github.com/bitshares/bitshares1-core/wiki/multisig)
* All sidechain specific functionalities will be implemented in sidechain handler.
* All general functionalities will be implemented in sidechain handler base class.

## Sequence diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing](https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing)

Same sequence diagram applies for all sidechain handlers. It shows interactions between components in a single sidechain handler.

