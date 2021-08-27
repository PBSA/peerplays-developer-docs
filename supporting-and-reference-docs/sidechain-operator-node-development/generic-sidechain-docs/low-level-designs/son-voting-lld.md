# son-voting-lld

## 1. Purpose

The purpose of this document is to provide low-level design for SON Voting mechanism.

## 2. Scope

The design requirement listed in this document will be limited to the voting mechanism of the SON Objects.

The document will also outline the way a SON object can be voted.

The document will also outline how the votes are tallied at the maintenance interval.

The document will also outline how the top N SONs are updated during the maintenance interval.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses.

As per the current implementation of Sidechain, a multi-sig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users.

Every Peerplays witnesses will have a bitcoin transaction signing key for this multi-sig bitcoin address and will be required to sign any withdrawal transaction.

When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multi-sig bitcoin address and the key of the incoming witness needs to be added.

The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. Sidechain Operator Node \( SONs \)

More on SON Objects can be found at [SON Objects and Operators](file:///C:/wiki/spaces/PIX/pages/333971489/SON+Objects+and+Operators)

## 5. How can SON be voted on

Users can vote for SONs in a similar way they vote for a witness.

A PeerPlays account updates his votes in '**account\_object**' via standard Graphene operation '**account\_update\_operation**' \(covered by '**account\_object::account\_options::num\_witness'** and '**account\_object::account\_options::votes'** fields\). wallet API has methods introduced for it: '**set\_desired\_witness\_committee\_son\_member\_count'** \(standard method, updated\) and '**vote\_for\_son'**.

Below is the sequence of how to vote for an SON.

## 6. Tallying Votes for SONs

At maintenance interval a '**vote\_tally\_helper::operator\(\)'** is called for each committee account on PeerPlays.

Votes for SON are added to '**database::\_vote\_tally\_buffer'**, using '**vote\_id'** as an index from '**account\_object::account\_options::votes'**, and '**voting\_stake'** of the account as value.

In '**database::\_son\_count\_histogram\_buffer'**, using '**amount\_son\_members / 2'** as an index, and '**voting\_stake'** of the account as value, the votes for a new number of top SONs are added.

Based on the above, the number of top SONs can only be odd, as during the calculation it is divided by 2, then multiplied by 2 and added + 1.

A method '**database::update\_active\_sons'** is called to determine the actual number of top SONs \(the order in which they are placed\).

First, a calculation of votes for the number of top SONs is performed \(step 2\). '**stake\_target'** needs to be reached, which equals 1/2 of the '**voting\_stake'** \(without considering the stakes of the votes for 0 SONs\).

The data from '**database::\_son\_count\_histogram\_buffer'** are orderly summed into a variable until it reaches '**stake\_target'**.

The index, at which the counting stops, will be multiplied by 2 and added + 1. The result of these operations will be the new number of top SONs.

New SONs are elected via '**database::sort\_votable\_objects\`** - a method to sort the witnesses and now SONs by the votes given to them by users.

A similar approach can be found in the witness voting tally code in **perform\_chain\_maintenance**.

