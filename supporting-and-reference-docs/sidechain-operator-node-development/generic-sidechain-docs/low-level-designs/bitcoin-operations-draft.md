# bitcoin-operations-draft

## **btc\_input\_data\_operation \(Same as bitcoin\_issue\_operation in Sidechain\)**

This operation creates an object that stores the information about deposits for their further approval. **Some minor changes will be required in its implementation i.e. the operation has to be signed by the active SONs holding more than 2/3rd of the votes.**

This operation contains the following fields:

| **Type** | **Name** | **Description** |
| :--- | :--- | :--- |
| asset | fee | transaction fee for the operation |
| account\_id\_type | payer | SON account |
| vector&lt;info\_for\_vin&gt; | info\_for\_vins | a vector of vins |
| string | btc\_block\_hash | hash of a block on the side of BTC |
| uint64\_t | estimated\_feerate | estimate fee for Bitcoin transaction |

This operation is handled as follows:

1\) Index is searched by id for vins same as the vin that is being added \( hash\( string\(hash\_of\_trx\) + string\(n\_vout\) \) \).  
2\) Id is calculated by the evaluator, since if it is passed in an operation, a potential attack vector opens: a valid ID for a different vin could be used to get approvals on another vin.  
3\) If nothing was found, a new btc\_input\_data\_object object is created, storing the number of active SON members. son\_id of the SON that sent the operation to set is also recorded in order to check confirmations by other SONs.  
4\) If set.size\(\) &gt; \(2/3 + 1\) \* active\_son\_members\_amount, btc\_input\_data\_object object is considered confirmed \(the consensus part\).  
5\) Estimated fee index is searched for btc\_fee\_data\_object by ID \( hash\( string\(btc\_block\_hash\) \) \).  
6\) If no object was found, we create a btc\_fee\_data\_object object, storing the number of active SON members.  
7\) Estimate fee map \(key=son\_id, value=fee\) is checked for records about operation sender. If there is a record, handling stops.  
8\) If there are no records found in step 7, new information is recorded to the map.  
9\) If map.size\(\) &gt; \(2/3 + 1\) \* active\_son\_members\_amount, btc\_fee\_data\_object is considered confirmed \(the consensus part\).

## **btc\_withrawal\_operation \(Same as withdraw\_pBTC\_operation in the existing Sidechain feature\)**

This operation creates an object with information about a BTC withdrawal \(btc\_withdrawal\_object\). **Some minor changes will be required in its implementation i.e. the operation has to be signed by the active SONs holding more than 2/3rd of the votes.**

The fields are as follows:

| **Type** | **Name** | **Description** |
| :--- | :--- | :--- |
| asset | fee | fee for the transaction on PeerPlays blockchain |
| account\_id\_type | payer | id of the PeerPlays account that initiates the withdrawal |
| string | data | bitcoin address, to which BTC is being withdrawn |
| uint64\_t | amount | the amount of withdrawal in satoshis |

This operation is handled as follows:

1\) Validation is performed to make sure that the withdrawing PeerPlays account has enough pBTC for the withdrawal amount + fees \(withdrawal amount + fee for son members + fee for Bitcoin transaction\).  
2\) btc\_withdrawal\_object is created. It contains the following information: ID of the withdrawing account \(account\_id\_type\), Bitcoin address to withdraw to \(string\), amount of withdrawal in satoshis \(uint64\_t\), used flag that indicates whether this object has been used \(bool\).  
3\) The amount from step 1 is burnt from the withdrawing user's balance  
4\) Confirmed objects are processed once every N block on PeerPlays \(where N is a constant specified in the protocol. An example of N is 300 - since average blockTime on Bitcoin is ~10 minutes, and ~2 seconds on PeerPlays\), which allows to minimize the tx fees by placing multiple withdrawals into one transaction.  
5\) On the node accepting a PeerPlays block a check is performed on its number \(block\_number % N == 0\). If block\_number = N, a btc\_transaction\_object, that contains a Bitcoin transaction. If inputs и withdrawals exceed the size of a Bitcoin transaction, another object is created containing a transaction that depends on the previous one.  
6\) On block acceptance an index storing the objects with btc\_tx is checked. If it cointains an unsigned transaction, a signal for creating a son\_member signature is emitted.  
7\) A Bitcoin transaction is emitted to Bitcoin blockchain once the object receives the required number of signatures.

## **btc\_transaction\_sign\_operation \(Same as btc\_transaction\_sign\_operation in the existing Sidechain feature\)**

This operation serves to sign Bitcoin transactions by SONs. **Some minor changes will be required in its implementation i.e. the operation has to be signed by the active SONs holding more than 2/3rd of the votes.** It contains the following fields:

| **Type** | **Name** | **Description** |
| :--- | :--- | :--- |
| asset | fee | fee for the transaction on PeerPlays blockchain |
| account\_id\_type | payer | SON account that pays the fee |
| vector&lt;vector&gt; | signatures | Signatures for vins |

The process for signing a transaction is as follows:  
1\) When a signal from database::apply\_block is called to create a signature for a Bitcoin transaction, a method from btc\_sidechain\_service is called.  
2\) btc\_transaction\_sign\_operation is created storing the signature, and is broadcast on PeerPlays blockchain. evaluator class for this operation records btc\_transaction\_object of the signature, and, when the required number of signatures is collected \(\(2 / 3 + 1\) \* son\_members\_amount\), sorts them in accordance with redeem script. The sorting is required since the keys in multisig script are handled in pre-defined order, and it is required that signatures are sorted in the same order \(key - signature\).

