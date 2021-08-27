# bitcoin-sidechain-multisig-bitcoin-wallet-and-bitcoin-addresses-pw

## Bitcoin sidechain Multisig Bitcoin Wallet and Bitcoin addresses \(PW\)

This doc describes the design of the multisignature Bitcoin wallet - \(Primary Wallet - hereinafter - PW\) mechanism as related to SON implementation, and the operations and objects concerning it.

PW is used to store all the BTC that has been deposited by users. The signatures included in it are the signatures of the accounts that are active SONs \(PeerPlays blockchain nodes with SON enabled and having enough vote weight to be included in Sidechain operations\) - therefore all the BTC in PW is under the control of active SONs. No SON can control the BTC stored there by himself, therefore a degree of decentralization is required.

Multisignature \(multisig\) refers to requiring more than one key to authorize a Bitcoin transaction. It is generally used to divide up responsibility for possession of Bitcoin.

Standard transactions on the Bitcoin network could be called “single-signature transactions,” because transfers require only one signature — from the owner of the private key associated with the Bitcoin address. However, the Bitcoin network supports much more complicated transactions that require the signatures of multiple people before the funds can be transferred. These are often referred to as M-of-N transactions \(where M is the number of signatures required for sending a transaction, and N is the number of signatures available in a wallet\). The idea is that Bitcoin becomes “encumbered” by providing addresses of multiple parties, thus requiring cooperation of those parties in order to do anything with them. These parties can be people, institutions or programmed scripts.

In our case, it is not enough to require M-of-N signatures to approve a transaction, as each signature comes with its own weight. This weight is determined by the number of votes received when voting for SONs. This requirement further complicates the bitcoin script, we cannot use the built-in OP\_CHECKMULTISIG operation, but have to check every signature separately and add up the corresponding weights.

The address of the PW is stored in btc\_address\_object.

### btc\_pw\_prevout\_object

This object is created when sending a transaction in Biotcoin network, and stores the information about PW vout. It is structured as follows:

| **Type** | **Name** | **Description** |
| :--- | :--- | :--- |
| prev\_out | vout | Information about the previous vout |
| sha256 | hash\_id | vout identificator |
| bool | confirmed | Flag that shows if the vout is confirmed |
| bool | used | Flag that shows if the vout has been used |

It stores the information on a PW vout with all the funds in it.

This object is created on each transaction from PW sent on Bitcoin blockchain.

Since all the funds on a PW are stored in one vout, it is required to be able to spend the money from an unconfirmed vout. Due to this fact, it is required to store the chain of PW vouts, from the last confirmed vout to the current vout. Due to the limitations of Bitcoin protocol the chain of vouts can not exceed 25. The chain of vouts is stored in btc\_pw\_prevout\_index. The index is represented as a chain of vouts from PW. After a Bitcoin transaction from PW gets N confirmations on Bitcoin blockchain, it is considered confirmed, therefore its vout becomes confirmed. The index is then updated, with the latest confirmed vout replacing the previous latest confirmed vout.

The standard value for the required number of confirmations is N=6, but many exchanges use N=3 for deposits. It is a trade-off between speed \(how long the conversion takes\) and safety. We would start with N=6, but as SONs are basically exchanges, we could change the requirement to only N=3 confirmations if desired.

## SideChain Bitcoin address

Bitcoin addresses are used to allow users to deposit Bitcoin to PeerPlays sidechain.

There are two types of Bitcoin addresses in PeerPlays network:

* PW-address - the storage of all BTC deposited by users \(described above\).
* Deposit-address - address generated for each user and used for depositing BTC to PeerPlays, after which the deposited BTC is sent to PW.

### Operation to create Bitcoin address

The operation is called btc\_address\_create\_operation. Its evaluator class creates an object called btc\_address\_object, containing a Bitcoin address. An address is a custom mutisig script consisting of keys and weights \(sum of votes for a SON\) SON.

### btc\_address\_create\_operation

This operation creates an object that stores the deposit address for an account \(btc\_address\_object\).

It includes the following fields:

| **Type** | **Name** | **Description** |
| :--- | :--- | :--- |
| asset | fee | The fee for executing the operation on PeerPlays blockchain |
| account\_id\_type | payer | Account that pays the fee for the operation |
| account\_id\_type | owner | Account to own the created address |

### btc\_address\_object

This object is created on execution of btc\_address\_create\_operationand stores the information about the address on Bitcoin blockchain.

It contains the following fields:

| **Type** | **Name** | **Description** |
| :--- | :--- | :--- |
| account\_id\_type | owner | Account that the Bitcioin deposit address belongs to |
| btc\_multisig\_segwit\_address | address | Information on the Bitcoin address |
| uint64\_t | count\_invalid\_pub\_key | The number of invalid co-sginatory keys in the address |

Active SONs are co-signatories for the funds stored on PW. Each SON has a public key on Bitcoin \(secp256k1\) and the weight of its signature.

This is executed on Bitcoin blockchain with the following script:

&lt;pubkey1&gt; OP\_CHECKSIG

OP\_IF

&lt;voting\_weight1&gt;

OP\_ELSE

0

OP\_ENDIF

OP\_SWAP

&lt;pubkey2&gt; OP\_CHECKSIG

OP\_IF

&lt;voting\_weight2&gt;

OP\_ADD

OP\_ENDIF

...

OP\_SWAP

&lt;pubkeyN-1&gt; OP\_CHECKSIG

OP\_IF

&lt;voting\_weightN-1&gt;

OP\_ADD

OP\_ENDIF

OP\_SWAP

&lt;pubkeyN&gt; OP\_CHECKSIG

OP\_IF

&lt;voting\_weightN&gt;

OP\_ADD

OP\_ENDIF

&lt;two\_thirds\_of\_total\_voting\_weight&gt;

OP\_GREATERTHAN

The point of the script is that each SON has a weight \(sum of votes for SON that is set at maintenance\). The script goes through each SON and checks their signature. If the signature is present, their weight is added to the variable at the top of the stack. At the end, the the sum is compared to 2/3 of the total weight of all SONs. Depending on the cumulative weight of votes a decision is made on whether the transfer is to be executed. For the transfer to happen the sum of weights in the script must be &gt;= 2/3 + 1 from the number of current active SON weights. All the variables in \`&lt;&gt;\` brackets change only when the set of SONs change, which is the same time as when the script is re-generated. Thus in the actual bitcoin script they will be literal constants.

Since the addresses depend on weights, and weights in turn depend on balances, on most occasions the deposit addresses will be re-generated after maintenance. There are two ways to handle this situation:

1. All pre-maintenance are to be considered invalid after maintenance.
2. Consider the pre-maintenance composition of SONs and their weights trusted post-maintenance. If the composition of SONs doesn't change, but their weights do, SON members are to be allowed to process the transaction in accordance with their pre-maintenance weights.

The maximum size of a Bitcoin script is 10kB, so with our limit of 15 SONs we will not approach this limit.

### SON downtime and communication problems, handling of invalid transactions

Potential issues could arise on both the side of Bitcoin and PeerPlays. In case a transaction is sent on Bitcoin and is not added to mempool, and, by extension, Bitcoin blockchain, it is vital to avoid the depositors losing their BTC.

Potential issues include:

1. Invalid serialization of a Bitcoin transaction.
2. Bitcoin transaction uses invalid signatures.
3. Bitcoin transaction uses invalid vin.

#### Detection of an invalid transaction

Before a transaction is sent to Bitcoin blockchain, SON memberplaces bitcoin\_transaction\_confirmations object with tx\_id to an index in btc\_sidechain\_net\_manager.

After receiving a block from Bitcoin, block counter bitcoin\_transaction\_confirmations::count\_block increments in these objects. As soon as the counter reaches confirmations\_num \(specified in chain\_parameters\) transaction confirmation validation takes place.

* Request to receive the number of confirmations \(method bitcoin\_rpc\_client::receive\_confirmations\_tx\). They are stored in bitcoin\_transaction\_confirmations::count\_block , re-recording the previous data.
* If the received number of confirmations &gt;= confirmations\_num, a transaction is considered confirmed.
* If the number of confirmations = 0, it means that the transaction has not been added to the blockchain, and it is required to check if it is in Bitcoin mempool.
* A request is made to check if a transaction is ino mempool \(bitcoin\_rpc\_client::receive\_mempool\_entry\_tx\).
* If it is in mempool, it is left as is, continuing to await confirmation.
* If it is not in mempool, it is considered invalid, or a fork has occurred on Bitcoin.

#### Handling of an invalid transactions

1. vins are checked for validity via bitcoin\_rpc\_client::receive\_confirmations\_tx.
2. All invalid vins are dropped.
3. All valid vins and vouts are added to btc\_revert\_operation and sent to the blockchain.
4. Transaction object is removed from sidechain\_net\_manager::bitcoin\_confirmations.

#### Handling btc\_revert\_operation

All valid objects btc\_input\_data\_object and btc\_withdrawal\_data\_object in the Evaluator class for this operation are marked as unused \(unspent\). Invalid btc\_input\_data\_object and btc\_transaction\_object are deleted.

Therefore only valid input and withdrawals will be used in transactions on Bitcoin.

#### btc\_revert\_operation

This operation is used to restore information about transfers and removal of invalid information.

It contains the following fields:

| **Type** | **Name** | **Description** |
| :--- | :--- | :--- |
| asset | fee | The fee for executing the operation on PeerPlays blockchain |
| account\_id\_type | payer | SON acount that pays for the operation |
| vector&lt;revert\_trx\_info&gt; | transactions\_info | Information on transactions, stored as a vector |

In the case of PeerPlays, potential issues arise when an active SON is disconnected and/or does not sign transactions due to the downtime, other SONs will need to sign transactions for him.

Borderline cases include:

* Active SONs with the cumulative weights &gt; 1/3 total active SONs weight do not sign transactions with deposit information or transactions outgoing from PW.
* A SON wishes to stop performing their functions due to the required maintenance on his end

In the first case the operations of Sidechain will not be performed and Bitcoin transactions will be reverted as described above.

In the second case the SON is required to utilize remove\_son\_operation\`. If they wish to return to the list of SONs and take part in SideChain operation, they will need to utilize create\_son\_operation\`.

## Primary wallet transfer during maintenance

When a new set of SONs is voted in, the primary wallet ownership has to be transferred to them. This involves multiple steps:

* Creating a new Bitcoin script using the new SON keys and weights
* Transferring any funds in the primary wallet to the new address

