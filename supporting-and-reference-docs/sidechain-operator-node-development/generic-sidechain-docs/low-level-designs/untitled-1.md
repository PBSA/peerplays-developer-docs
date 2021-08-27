# creation of a multi-sig bitcoin address lld

## 1. Purpose

The purpose of this document is to provide a low-level design for the creation of a multi-signature bitcoin address.

## 2. Scope

The design requirement listed in this document will be limited to the creation of a multi-signature address managed by SONs in the Peerplays network.

The document will also outline the types of transactions present in the bitcoin network.

The document will also outline the scripting mechanism that locks/unlocks the transactions in the bitcoin network.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses.

As per the current implementation of Sidechain, a multi-sig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users.

Every Peerplays witnesses will have a bitcoin transaction signing key for this multi-sig bitcoin address and will be required to sign any withdrawal transaction.

When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multi-sig bitcoin address and the key of the incoming witness needs to be added.

The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. Types of Transactions

### 4.1 P2PK

They are known as Pay to PubKey transactions.

They are an older way of sending bitcoins to a single address.

“OP\_DATA\_65 OP\_CHECKSIG” and “OP\_DATA\_33 OP\_CHECKSIG” is the format of the Pkscript used in these types of transactions.

The public key of the receiver of bitcoin is specified in the locking Pkscript.

Unlocking sigscript should contain the signature with the private key of the spender/receiver.

### 4.2 P2PKH

They are known as Pay to PubKey Hash transactions.

They are the standard and popular way of sending bitcoins to a single address.

“OP\_DUP OP\_HASH160 OP\_DATA\_20 OP\_EQUALVERIFY OP\_CHECKSIG” is the format of the Pkscript in these types of transactions.

The hash160 of the public key of the receiver is specified in the locking script.

### 4.3 P2SH

They are known as Pay to Script Hash transactions.

They are generally used to send bitcoins to multi-sig addresses.

“OP\_HASH160 OP\_DATA\_20 OP\_EQUAL” is the format of the Pkscript in these types of transactions.

The OP\_DATA\_20 opcode is followed by a 20 byte hash of the P2SH RedeemScript which can be provided by the receiver in a future transaction.

It moves the responsibility for supplying the conditions to redeem a transaction from the creator of the transaction to the receiver.

### 4.4 P2WPKH

They are known as Pay To Witness Public Key Hash transactions.

They are introduced as part of the SegWit improvements to the bitcoin network.

“0 &lt;20-byte-PublicKeyHash&gt;” is the format of the Pkscript in these types of transactions.

The ScriptSig/Unlocking-script is empty.

“&lt;Signature&gt; &lt;PublicKey&gt;” is the format of the witness field.

Verification goes as follows,

* Check that the witness contains exactly two items.
* Verify that HASH160 of the witness' public key is equal to the one present in the witness program.
* Verify the signature as &lt;Signature&gt; &lt;PublicKey&gt; OP\_CHECKSIG.

There is no OP\_CHECKSIG operand in neither PKscript/scriptPubKey nor witness, it is implicit by the definition of P2WPKH.

### 4.5 P2WSH

They are known as Pay To Witness Script Hash transactions.

They are introduced as part of the SegWit improvements to the bitcoin network.

“0 &lt;32-byte-redeemScriptHash” is the format of the Pkscript in these types of transactions.

The ScriptSig/Unlocking-script is empty.

“&lt;witness items&gt; &lt;redeemScript&gt;” is the format of the witness field.

Verification goes as follows,

* The last item in the witness is popped off, hashed with SHA256, compared against the 32-byte-hash in Pkscript/scriptPubKey.
* Then the script is executed with the remaining data from the witness.

### 4.6 Others

There are nested types of transactions as well like P2WSH nested in P2SH and P2WPKH nested in P2SH.

These are out of the scope of this document and can be found online.

## 5. Bitcoin Scripting

Bitcoin uses a scripting system for transactions.

The scripts are simple, stack-based and processes from left to right.

They are not Turing-complete and don’t have loops.

A script is essentially a list of instructions recorded with each transaction that describes how the next person wanting to spend the Bitcoins being transferred can gain access to them.

The script for a typical Bitcoin transfer to a destination Bitcoin address simply encumbers future spending of the bitcoins with two things: the spender must provide

1. a public key that, when hashed, yields destination address embedded in the script, and
2. a signature to prove ownership of the private key corresponding to the public key just provided.

As it is stack-based, a transaction is valid if nothing in the combined script \(i.e. Pkscript+Scriptsig\) triggers failure and the top stack item is True \(non-zero\) when the script exits.

### 5.1 P2PKH Script Execution

Following is the script for a Typical P2PKH transaction,

scriptPubKey: OP\_DUP OP\_HASH160 &lt;pubKeyHash&gt; OP\_EQUALVERIFY OP\_CHECKSIG

scriptSig: &lt;sig&gt; &lt;pubKey&gt;

Note: scriptSig is in the input of the spending transaction and scriptPubKey is in the output of the previously unspent i.e. "available" transaction.

| **Stack** | **Script** | **Description** |
| :--- | :--- | :--- |
| Empty. | &lt;sig&gt; &lt;pubKey&gt; OP\_DUP OP\_HASH160 &lt;pubKeyHash&gt; OP\_EQUALVERIFY OP\_CHECKSIG | scriptSig and scriptPubKey are combined. |
| &lt;sig&gt; &lt;pubKey&gt; | OP\_DUP OP\_HASH160 &lt;pubKeyHash&gt; OP\_EQUALVERIFY OP\_CHECKSIG | Constants are added to the stack. |
| &lt;sig&gt; &lt;pubKey&gt; &lt;pubKey&gt; | OP\_HASH160 &lt;pubKeyHash&gt; OP\_EQUALVERIFY OP\_CHECKSIG | Top stack item is duplicated. |
| &lt;sig&gt; &lt;pubKey&gt; &lt;pubHashA&gt; | &lt;pubKeyHash&gt; OP\_EQUALVERIFY OP\_CHECKSIG | Top stack item is hashed. |
| &lt;sig&gt; &lt;pubKey&gt; &lt;pubHashA&gt; &lt;pubKeyHash&gt; | OP\_EQUALVERIFY OP\_CHECKSIG | Constant added. |
| &lt;sig&gt; &lt;pubKey&gt; | OP\_CHECKSIG | Equality is checked between the top two stack items. |
| true | Empty. | Signature is checked for the top two stack items. |

### 5.2 P2SH Script Execution

P2SH script is a bit different from a typical P2PKH transaction script.

Following is an example of a 2-of-3 multisig address transaction,

scriptPubKey: OP\_HASH160 &lt;redeemScript\_hash&gt; OP\_EQUAL

scriptSig: 0 &lt;sig1&gt; &lt;sig2&gt; &lt;redeemScript&gt;

redeemScript: 2 &lt;pub-key1&gt; &lt;pub-key2&gt; &lt;pub-key3&gt; 3 OP\_CHECKMULTISIG

The flow of execution is something like this for P2SH,

1. Execute scriptSig which creates stack
2. Copy stack to stackCopy
3. Execute scriptPubKey using stack
4. If P2SH, replace stack with stackCopy
5. If P2SH, Pop top item off of stack and execute as redeemScript using stack.

Essentially, scriptPubKey is executed and it checks if the hash of redeemScript is that of the hash given.

Then, redeemScript is executed with the required signatures in front.

Execution for both **P2WSH** and **P2WPKH** is similar to the related non-segwit variants except that scriptSig is empty and we keep the data in witness script.

### 5.3 A bit about OP\_CHECKMULTISIG

This crypto opcode is used to verify the signature for m-of-n multisig address transactions.

It handles up to 20-of-20 multisig address transactions.

It has a bug in that it tries to pop m+n+3 elements off the stack instead of m+n+2 elements.

That’s why a zero \(0\) is added at the front of scriptSig/Witness.

A high-level pseudo-code looks like this,

1. Pop n off of the stack \(number of public keys\)
2. Pop n public keys off of the stack.
3. Pop m off of the stack \(number of required signatures\)
4. Pop m signatures off of the stack.
5. Pop one more element off of the stack, and ignore it. \(This is a bug, but it can't be fixed because this is consensus-critical code.\)
6. Loop through all of the public keys, starting with the keys at the top of the stack.
   1. For each public key, check a single signature.
   2. For the first public key checked, start with the signature closest to the top of the stack.
   3. If it fails to verify, go to the next public key and check the same signature.
   4. If it succeeds, go to the next public key with the next signature.

Note that the signatures need to be in the same order as the key's that they're signing for.

1. If all of the signatures succeeded with one of the keys, CHECKMULTISIG returns 1, otherwise 0.

Essentially, the key takeaways are,

* All the signatures should be in the same order as that of public keys.
* All of the signatures provided should match with at least one of the public keys.

### 5.4 Why can’t we use OP\_CHECKMULTISIG?

We can’t use the opcode because it assumes by default all signatures/public-keys have the same weight.

It just checks if we have m valid signatures out of n possible public keys.

But in the SON’s case, we have weights attached to each SON based on the votes a SON receives.

We have to in SON’s case, add up the weights of each SON signature and check if we have enough total weight to publish a transaction on the bitcoin network.

So we can’t use a simple OP\_CHECKMULTISIG in our case; we have to come up with a custom script.

## 6. Custom Bitcoin Script for SONs

We can go with P2WSH or P2SH for our wallet addresses.

But P2WSH is preferred as it is the latest, efficient and backward compatible.

For P2WSH, we have to provide witness script and a hash of our redeemScript to depositor/sender.

witness = “&lt;witness items&gt; &lt;redeemScript&gt;”

redeemScript can be as follows,

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

witness items is a list of SON signatures in the ascending order of their voting weights assuming the redeem script has public keys of SONs in descending order.

We will have to order the SON signatures in the reverse order of how public keys are ordered in redeem script.

Even if any SON signature is not available, we have to add a dummy signature in order to execute the script properly.

## 7. Design Changes

## 8. Change of scenario of SONs

### 8.1 Change of SON Weights

SON weights are calculated based on the votes received.

This calculation is done at the end of the maintenance interval.

Of the active SONs\(15\) selected after the maintenance interval, we can treat their respective weights as total\_votes from each son\_object.

Eg. Take 3 SONs,

SON1 weight → total\_votes = 10

SON2 weight → total\_votes = 20

SON3 weight → total\_votes = 30

Total weight = 60

Minimum required weight to unlock the transaction = 2/3 \* 60 + 1 = 41

If the rankings remain the same but the weights change, we can’t use the same script as before because weights in the script are invalid now.

If the rankings change but the same SONs are active, we still can’t use the same script because weights in the script are invalid now.

### 8.2 SON Down

If any of the SONs are down and the current total weight is greater than or equal to 67% of the actual total weight of SONs at the start of the maintenance interval, we can still continue to sign transactions hoping that we hit the required limit in the script.

Eg. Take 5 SONs,

SON1 weight → 20

SON2 weight → 20

SON3 weight → 20

SON4 weight → 20

SON5 weight → 20

Total Weight when address is created → 100

SON5 went down abruptly,

Current total weight → 80 \( &gt;= 67 \)

So we can still continue to work with the existing set of addresses till the next maintenance.

Now, SON4 also went down abruptly,

Current total weight → 60 \( &lt; 67 \)

Now we are below the required minimum weight in the script, so we can’t successfully sign any transactions.

More down scenarios are captured [here.](https://peerplays.atlassian.net/wiki/spaces/PIX/pages/337870860)

