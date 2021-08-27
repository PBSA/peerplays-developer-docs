# son-consensus-communication-and-transaction-signing-on-chain-lld

## 1.Purpose

The purpose of this document is to provide low-level design for the communication mechanism between SONs and transaction signing.

## 2. Scope

The design requirement listed in this document will be limited to the communication, consensus and transaction signing of SONs on-chain.

The document also proposes a design to create a way to know the health of an active SON.

The document takes only the Bitcoin sidechain into consideration as of this writing but can be extended to others in the future.

The document doesn’t deal with sidechain deposit handling or withdrawal.

The assumption is that we use a single PW design without creating deposit addresses.

Note1: For the simplicity of the examples, let's assume we have 3 SONs.

Note2: Code snippets provided in the document are pseudo-codes, they just illustrate basic logic and may not be syntactically correct.

### 2.1 Glossary

_**Consensus-SON**_**:** Top 2/3rd active SONs who signs transactions on behalf of the 15 active SONs takes part in the consensus and the operations. They are termed as Consensus-SONs

**SON-Quorum**: The availability of SONs with 2/3rd accumulated stakes via votes is a SON-Quorum.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses.

As per the current implementation of Sidechain, a multi-sig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users.

Every Peerplays witnesses will have a bitcoin transaction signing key for this multi-sig bitcoin address and will be required to sign any withdrawal transaction.

When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multi-sig bitcoin address and the key of the incoming witness needs to be added.

The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. Basic Idea

One of the biggest challenges of the SON project is to achieve consensus among the SONs on the blockchain.

The consensus is required among SONs to perform the following tasks,

1. Transferring BTC to User address after withdrawal.
2. Transferring BTC from old PW to new PW.
3. To issue pBTC.
4. To report on another active SON for being inactive.

Also, the challenge is that SONs have to make sure they do all the work with respect to the sidechain and relieve the witnesses of the extra load of listening to the sidechain and handling deposits and withdrawals.

But the records/statistics of the sidechain transactions, issuance of pBTC, SON health status, etc have to be kept on-chain.

The Statics of the sidechain transactions, issuance of pBTC, SON health, availability of SONs etc have to be kept on the Peerplays blockchain.

This provides a robust mechanism for anyone to get the historical performance of the SONs, near real time view of the chain fund movements.

The basic idea of how to achieve consensus is as follows,

1. Create a peerplays account for a sidechain [SON-195](https://peerplays.atlassian.net/browse/SON-195) - Getting issue details... STATUS , let's name it ‘son-btc-account’.
2. Owners for the account are SONs.
3. Weights of their signatures are proportional to the votes they receive.
4. The weight threshold of the account is 2/3 of the total weight.
5. Any operation related to the SON tasks has the paying account as the son account that's created.
6. All such operations are encapsulated in proposals.
7. So for any SON proposal to be successfully executed it needs to have 2/3 of the total authority of SONs.

Eg. SON1 weight = 7  
SON2 weight = 5  
SON3 weight = 3  
Total weight = 15  
Weight Threshold = 2/3 \* 15 = 10

So signatures of one of the SON combinations \(1,2\), \(1,3\), \(1,2,3\) are required to execute a successful transaction/proposal.

Psuedo-code is as follows,

const auto& son\_btc\_account = create&lt;account\_object&gt;\( \[&\]\( account\_object& obj \) {

obj.name = "son-btc-account";

obj.statistics = create&lt;account\_statistics\_object&gt;\(\[&\]\( account\_statistics\_object& acc\_stat \){ acc\_stat.owner = obj.id; }\).id;

obj.owner.weight\_threshold = 10;

obj.active.weight\_threshold = 10;

obj.membership\_expiration\_date = time\_point\_sec::maximum\(\);

obj.network\_fee\_percentage = GRAPHENE\_DEFAULT\_NETWORK\_PERCENT\_OF\_FEE;

obj.lifetime\_referrer\_fee\_percentage = GRAPHENE\_100\_PERCENT - GRAPHENE\_DEFAULT\_NETWORK\_PERCENT\_OF\_FEE;

// Add Authority Weights

obj.owner.add\_authority\( son1, 7 \);

obj.active.add\_authority\( son1, 7 \);

obj.owner.add\_authority\( son2, 5 \);

obj.active.add\_authority\( son2, 5 \);

obj.owner.add\_authority\( son3, 3 \);

obj.active.add\_authority\( son3, 3 \);

}\);

## 5. How to get SON Public Keys for Multi-Sig PW creation

We need SON public keys to create a multi-sig Bitcoin address for PW.

We get this from son\_object::signing\_key stored in the database.

The key is assigned during the registration of SON through CLI \(refer to create\_son\).

We can also extend the create\_son API to include a sidechain public key that is different from peerplays public key. [SON-190](https://peerplays.atlassian.net/browse/SON-190) - Getting issue details... STATUS

## 6. Creation and Signing of BTC Transaction

As discussed above, we need to egress BTC transactions in case of transferring BTC to a user after withdrawal and transferring BTC from old PW to new PW.

Following steps will accomplish this,

1. A Proposal is created with the fee-paying account as the individual SON account who is proposing.
2. Create the BTC raw transaction.
3. Create a BTC Transaction Send Operation with fee payer as the peerplays bitcoin sidechain account son-btc-account\(weight threshold as 2/3 of SONs total weight\).
4. Encapsulate the send operation in the proposal created.
5. Ensure the proposal lifetime is sufficient \( N\*\(number of SONs\)\*\(block\_creation\_interval\) where N is configurable i.e. 1,2,3,4 etc\).
6. Publish the proposal on to the peerplays blockchain.
7. Each SON after receiving the proposal validated the data against the sidechain BTC database stored in the plugin locally.
8. If appropriate, each SON signs the BTC transaction.
9. Create a BTC Transaction Sign Operation with fee payer as the individual SON account who is signing \(i.e. SON1 account, SON2 account, etc\).
10. Sign the BTC transaction and store the signature in the sign operation.
11. Encapsulate the operation in a peerplays transaction, sign it and send it to peerplays blockchain.
12. The sign operation is evaluated by checking the SON signature on BTC Transaction with the SON public key registered on peerplays blockchain.
13. If evaluation succeeds, during the do\_apply of sign operation the verified signatures are stored in the send operation encapsulated in the proposal created initially.
14. After the required signatures are available or lifetime of the proposal is expired and if enough signatures to reach the weight threshold of the son-btc-account are present, the proposal is executed, signatures are sorted and BTC transaction is sent out.

As the weights in the bitcoin script and weights of the son-btc-account are the same, if proposal is executed successfully the bitcoin transaction will also be successful.

Creating send-BTC transaction proposal pseudo-code,

// Create BTC Transaction with the vins and vouts stored in SON Database

btc\_tx = create\_btc\_transaction\(\);

// Create BTC Transaction Send Operation

bitcoin\_transaction\_send\_operation btc\_send\_op;

btc\_send\_op.payer = son-btc-account;

// Get the BTC vector of inputs

btc\_send\_op.vins = btc\_tx.get\_valid\_vins\(\);

// Fill the BTC vector of outputs

btc\_send\_op.vout = fill\_vouts\(\);

// Store BTC Transaction data i.e. hash, amount, fee etc.

btc\_send\_op.transaction = btc\_tx;

// Create the proposal and encapsulate the send operation in it.

proposal\_create\_operation proposal\_op;

proposal\_op.fee\_paying\_account = son\_account;

proposal\_op.proposed\_ops.push\_back\( op\_wrapper\( btc\_send\_op \) \);

uint32\_t lifetime = \( get\_global\_properties\(\).parameters.block\_interval \* get\_global\_properties\(\).active\_sons.size\(\) \) \* 4;

proposal\_op.expiration\_time = time\_point\_sec\( head\_block\_time\(\).sec\_since\_epoch\(\) + lifetime \);

Each SON signing the send-BTC transaction proposal pseudo-code,

bitcoin\_transaction\_sign\_operation sign\_operation;

sign\_operation.payer = current\_son.son\_account;

// Proposal ID of the send-BTC Transaction Operation

sign\_operation.proposal\_id = proposal\_id;

// Assign SON ID

sign\_operation.son\_id = my\_son\_id;

// Get the vins from the proposal

auto vins = btc\_send\_op.vins;

// Sign the transaction

sign\_operation.signatures = sign\_transaction\(btc\_send\_op.transaction, vins\);

BTC Sign Operation Evaluator pseudo-code,

void\_result bitcoin\_transaction\_sign\_evaluator::do\_evaluate\( const bitcoin\_transaction\_sign\_operation& op \) {

const auto& proposal\_itr = proposal\_idx.find\(op.proposal\_id\);

FC\_ASSERT\(proposal\_idx.end\(\) != proposal\_itr, "proposal not found"\);

FC\_ASSERT\(is\_signed\_by\_active\_son\(op.son\_id\), "Not signed by active SON."\);

const auto& son\_object = get\_son\_object\(op.son\_id\);

bytes public\_key\( public\_key\_data\_to\_bytes\( son\_object.signing\_key.key\_data \) \);

auto btc\_send\_op = proposal\_itr-&gt;proposed\_transaction.operations\[0\].get&lt;bitcoin\_transaction\_send\_operation&gt;\(\);

// Get the vector of input txs from send-operation from proposal and verify sigs with SON Public key

auto vins = btc\_send\_op.vins;

FC\_ASSERT\( check\_sigs\( public\_key, op.signatures, vins, btc\_send\_op.transaction \) \);

}

void\_result bitcoin\_transaction\_sign\_evaluator::do\_apply\( const bitcoin\_transaction\_sign\_operation& op \) {

database& d = db\(\);

// Get the send-BTC Transaction Operation Proposal created

const auto& proposal = op.proposal\_id\( d \);

// Modify the operation by adding the validated signature

d.modify\( proposal, \[&\]\( proposal\_object& po \) {

auto bitcoin\_transaction\_send\_op = po.proposed\_transaction.operations\[0\].get&lt;bitcoin\_transaction\_send\_operation&gt;\(\);

for\( size\_t i = 0; i &lt; op.signatures.size\(\); i++ \) {

bitcoin\_transaction\_send\_op.transaction.vin\[i\].scriptWitness.push\_back\( op.signatures\[i\] \);

}

po.proposed\_transaction.operations\[0\] = bitcoin\_transaction\_send\_op;

}\);

// Create proposal\_update\_operation and add the SON's approval to the proposal.

proposal\_update\_operation update\_op;

update\_op.fee\_paying\_account = op.payer;

update\_op.proposal = op.proposal\_id;

update\_op.active\_approvals\_to\_add = { op.payer };

d.apply\_operation\(\*trx\_state, update\_op\);

return void\_result\(\);

}

BTC Send Transaction Operation Evaluator Pseudo-code,

void\_result bitcoin\_transaction\_send\_evaluator::do\_evaluate\( const bitcoin\_transaction\_send\_operation& op \) {

// Check if the payer of the operation is the multi signatory SON BTC peerplays account.

FC\_ASSERT\( son-btc-account == op.payer \);

return void\_result\(\);

}

object\_id\_type bitcoin\_transaction\_send\_evaluator::do\_apply\( const bitcoin\_transaction\_send\_operation& op \) {

bitcoin\_transaction\_send\_operation& mutable\_op = const\_cast&lt;bitcoin\_transaction\_send\_operation&&gt;\( op \);

database& d = db\(\);

// Make the transaction ready by sorting sigs etc

finalize\_btc\_tx\(mutable\_op\);

auto new\_vins = create\_transaction\_vins\( mutable\_op \);

// Create the bitcoin\_transaction\_object to be stored on the chain.

const bitcoin\_transaction\_object& btc\_tx = d.create&lt; bitcoin\_transaction\_object &gt;\( \[&\]\( bitcoin\_transaction\_object& obj \)

{

obj.pw\_vin = mutable\_op.pw\_vin.identifier;

obj.vins = new\_vins;

obj.vouts = mutable\_op.vouts;

obj.transaction = mutable\_op.transaction;

obj.transaction\_id = mutable\_op.transaction.get\_txid\(\);

obj.fee\_for\_size = mutable\_op.fee\_for\_size;

}\);

// Send out the transaction.

send\_bitcoin\_transaction\( btc\_tx \);

return btc\_tx.id;

}

## 7. SON Heart Beats

Heartbeat transactions are sent by active SONs to notify the blockchain that they are active.

The frequency of the heartbeat can be configured through chain parameter extensions.

We need these in order to mark active SONs as in\_maintenance if heartbeats are missed.

Evaluation and application of the heartbeat operation pseudo-code,

void\_result son\_evaluator::do\_evaluate\(const son\_heartbeat\_operation& op\) {

// Get SON Object from son id through son\_index

son\_object son = get\_son\_by\_id\(op.son\_id\);

// Payer for hearbeat operations are individual SON accounts

FC\_ASSERT\(op.payer == son.son\_account\);

return void\_result\(\);

}

void\_result son\_evaluator::do\_apply\(const son\_heartbeat\_operation& op\) {

// Get SON Object from son id through son\_index

son\_object son = get\_son\_by\_id\(op.son\_id\);

// Get SON Statistics Object from son\_stats\_index

son\_statistics\_object son\_stats = get\_son\_stats\(son.statistics\);

// Update the last active timestamp

db.modify\( son\_stats, \[&\]\( son\_statistics\_object& \_s\)

{

\_s.last\_active\_ts = op.ts;

}\);

}

## 8. Reporting SON Down

If any SON misses N number of heartbeats then one of the other active SONs can notify the peerplays blockchain about it.

N can be configurable with chain parameter extensions.

One of the rest of the active SON can raise a son\_deactivate\_operation to report on the SON which is down.

The fee-paying account for this operation should be the peerplays bitcoin sidechain account son-btc-account.

A proposal is created and the deactivate operation is encapsulated inside the proposal. \(Fee-paying account for the proposal is individual SON account\)

The lifetime of the proposal can be N\*\(number of SONs\)\*\(block\_creation\_interval\) where N is configurable i.e. 1,2,3,4 etc.

The rest of the SONs on receiving the proposal can validate and add their approvals to the proposal.

After the required signatures are available or once the proposal is expired, if it succeeds it is executed and the down SON status is set to in\_maintenance on the blockchain.

If the down SON becomes active and sends a heartbeat while the deactivate proposal is active, we can leave the SON status as active but add up the downtime in statistics object.

Evaluation and application of the son deactivate operation pseudo-code,

void\_result son\_evaluator::do\_evaluate\(const son\_deactivate\_operation& op\) {

// Get SON Object from son id through son\_index

son\_object son = get\_son\_by\_id\(op.son\_id\);

// Payer for hearbeat operations are individual SON accounts

FC\_ASSERT\(op.payer == son.son\_account\);

return void\_result\(\);

}

void\_result son\_evaluator::do\_apply\(const son\_deactivate\_operation& op\) {

// Get SON Object from son id through son\_index

son\_object son = get\_son\_by\_id\(op.son\_id\);

// Get SON Statistics Object from son\_stats\_index

son\_statistics\_object son\_stats = get\_son\_stats\(son.statistics\);

// Update the last active timestamp

db.modify\( son, \[&\]\( son\_object& \_son\_obj\)

{

\_son\_obj.status = son\_status::in\_maintenance;

}\);

db.modify\( son\_stats, \[&\]\( son\_statistics\_object& \_s\)

{

\_s.last\_down\_ts = op.ts;

}\);

}

Adding approvals by rest of the active SONs pseudo-code,

// Create proposal\_update\_operation and add the SON's approval to the proposal.

proposal\_update\_operation update\_op;

update\_op.fee\_paying\_account = op.payer;

update\_op.proposal = op.proposal\_id;

update\_op.active\_approvals\_to\_add = { op.payer };

d.apply\_operation\(\*trx\_state, update\_op\);

## 9. SON Issue pBTC

Once a required number of confirmations are reached for a transaction sending BTC from user bitcoin address to PW address, SON pBTC issue operation has to be raised to user’s peerplays account.

As discussed above similar proposal process is followed.

1. A proposal is created by one active SON and the fee-paying account is set to the individual account of the SON. \(lifetime same as discussed above\)
2. A bitcoin\_issue\_operation is created for which the fee-paying account is set to son-btc-account.
3. The issue operation is encapsulated in the proposal and sent to the peerplays blockchain.
4. Rest of the active SONs validate their plugin BTC databases and add approvals upon receiving the proposal.
5. Once the required number of approvals \(2/3 of SONs total weight\) are added, proposal is executed and the pBTC is issued.

SON Issue pBTC pseudo-code,

void\_result son\_evaluator::do\_evaluate\(const bitcoin\_issue\_operation& op\) {

// Check if the payer of the operation is the multi signatory SON BTC peerplays account.

FC\_ASSERT\( son-btc-account == op.payer \);

// Check if the vin tx input address is user registered address on peerplays blockchain.

FC\_ASSERT\( is\_address\_registered\(op.vin\_addres\)\)

return void\_result\(\);

}

void\_result son\_evaluator::do\_apply\(const bitcoin\_issue\_operation& op\) {

// Get the peerplays account to issue pBTC

const auto& account\_to\_issue = get\_account\_to\_issue\(op.vin\_address\);

// Get the amount to issue i.e. equal to BTC amount transferred

const auto& amount\_to\_issue = get\_amount\_to\_issue\(op.amount\);

// Follow the normal UIA issue procedure.

add\_issue\(account\_to\_issue, amount\_to\_issue\);

}

Adding approvals by rest of the active SONs pseudo-code,

// Create proposal\_update\_operation and add the SON's approval to the proposal.

proposal\_update\_operation update\_op;

update\_op.fee\_paying\_account = op.payer;

update\_op.proposal = op.proposal\_id;

update\_op.active\_approvals\_to\_add = { op.payer };

d.apply\_operation\(\*trx\_state, update\_op\);

## 10. Choosing the SON who creates the next proposal

In all the above operations, any one of the SON has to raise the initial proposal.

If multiple proposals are raised for the same BTC transaction, only one of them is valid and the rest of them should be rejected.

To achieve this we can use one of the existing witnesses shuffled or scheduled algorithms.

With these algorithms in place, all the SONs know when is their slot to publish a proposal for the next event.

The detailed design for this is out of the scope of this document and should be part of SON plugin handling.

For the existing witness scheduling algorithm please refer to [this link.](https://github.com/PBSA/peerplays/blob/master/libraries/chain/db_witness_schedule.cpp)

## 11. Data Structures

### 11.1 bitcoin\_transaction\_send\_operation

We can retain most of the implementation from the sidechain branch.

It can be found [here.](https://github.com/peerplays-network/peerplays/blob/feature-sidechain/libraries/chain/include/graphene/chain/protocol/bitcoin_transaction.hpp)

### 11.2 bitcoin\_transaction\_sign\_operation

We can retain most of the implementation from the sidechain branch.

It can be found [here.](https://github.com/peerplays-network/peerplays/blob/feature-sidechain/libraries/chain/include/graphene/chain/protocol/bitcoin_transaction.hpp)

### 11.3 son\_heartbeat\_operation

### 11.4 son\_deactivate\_operation

## 12. SON changes during Maintenance

One of the many things we have to do during maintenance is updating the PW and the multi signatory SON account \(son-btc-account\).

As discussed above the weights of signatures is the same for both PW and the multi signatory SON account, we have to change both accordingly after the SON weight changes or SON changes.

The change of PW is discussed [here.](https://peerplays.atlassian.net/wiki/spaces/PIX/pages/351895635/Bitcoin+Multi+Signature++Wallet+-++LLD)

Eg. Weights of the SONs have changed from the previous example above and a new SON is active.  
SON1 weight = 10  
SON2 weight = 5  
SON4 weight = 6  
Total weight = 21  
Weight Threshold = 2/3 \* 21 = 14

So signatures of one of the SON combinations \(1,2\), \(1,4\), \(1,2,4\) are required to execute a successful transaction/proposal.

For changing the weights of peerplays SON account following is the pseudo-code,

modify\( sidechain\_account, \[&\]\( account\_object& obj \) {

// Add new weight threshold

obj.owner.weight\_threshold = 14;

obj.active.weight\_threshold = 14;

// Add new authority for SON1

obj.owner.add\_authority\( son1, 10 \);

obj.active.add\_authority\( son1, 10 \);

// Add new authority for SON2

obj.owner.add\_authority\( son2, 5 \);

obj.active.add\_authority\( son2, 5 \);

// Remove the SON3 Authority

obj.owner.add\_authority\( son3, 0 \);

obj.active.add\_authority\( son3, 0 \);

// Add new authority for SON4

obj.owner.add\_authority\( son4, 6 \);

obj.active.add\_authority\( son4, 6 \);

}\);

## 13. Conclusion

This approach can greatly relieve the peerplays blockchain of any type of sidechain listening, deposit, and withdrawal operations.

Only the transactions which are needed to be logged are processed by the nodes other than SONs.

All the operations which need multiple signatures can directly be accepted by everyone on the blockchain as the proposals ensure all signatures present before execution.

The most important aspect of this design is we can re-use most of the existing bitcoin-related sidechain code which can reduce our development effort.

