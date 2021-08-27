# claiming initial son vesting lld

## 1. Purpose

The purpose of this document is to provide low-level design for the process of claiming the initial vesting amount to become SON on PPY Blockchain.

## 2. Scope

The design requirement listed in this document will be limited to the de-registration of SON Node from active SONs and claiming the initial vesting amount by a user.

The document will also outline the function flow for how de-registration of SON node happens.

The document will also outline the new chain parameters required to configure the initial vesting amount and vesting period duration after de-registration happens.

The document will also outline the new data structures required to control the user from claiming the initial vesting amount before the vesting period duration expires.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses.

As per the current implementation of Sidechain, a multi-sig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users.

Every Peerplays witnesses will have a bitcoin transaction signing key for this multi-sig bitcoin address and will be required to sign any withdrawal transaction.

When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multi-sig bitcoin address and the key of the incoming witness needs to be added.

The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\).

The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. Current Vesting scenario for GPOS

As proposed and implemented in GPOS, user can vest a certain amount to,

1. Participate in voting for proposals.
2. To earn a part of commissions for transactions.

The vesting period is currently 6 months and it is divided into sub-periods of 1 month.

At the end of every sub-period, a parameter called Vesting Factor \(typically between 0 to 1\) is calculated based on user participation.

The Vesting Factor decreases if a user doesn't participate in any voting activity for the preceding sub-period.

Users can retain their starting Vesting Factor of 1 if they continue to participate in voting in every sub-period.

And commissions are paid out proportional to the Vesting factor.

Any commission which is not paid to users because of their inactivity is transferred into a separate account which can be used further.

## 5. Vesting Requirement for SON

As proposed in the Functional specification of SON, a user who wants to become an SON should vest a certain amount \(50 PPY\) in a separate account.

The vested amount would be locked indefinitely until 2 days after the SON is deregistered on the network.

The vesting amount doesn't change and rewards are paid into liquid/normal account of the user.

## 6. Perceived Challenges for implementing Vesting for SON

As seen above for GPOS, at the time of the creation of Vesting Account a vesting period of at least one sub-period can be set to prevent users from withdrawing the vesting amount.

So when a user tries to withdraw his vesting amount, a vesting policy which was initialized during the creation of the vesting account can be used to decline the withdrawal.

However in SON Vesting at the time of creation, we can't initialize a policy with a particular duration that can decline user withdrawals.

A policy cannot be initialized at the time of Vesting account creation because the vesting period depends on the time of deregistration of SON.

## 7. Current list of Vesting policies

Currently, there are two types of vesting policies are in place,

### 7.1 linear\_vesting\_policy

This vesting policy is used to mimic traditional stock vesting contracts where each day a certain amount vests until it is fully matured.

No amount can be withdrawn before a certain number of cliff seconds have elapsed.

### 7.2 cdd\_vesting\_policy

This policy defines vesting in terms of coin-days accrued which allows for dynamic deposit/withdraw.

The economic effect of this vesting policy is to require a certain amount of interest to accrue before the full balance may be withdrawn.

Interest accrues as coin days = \(balance \* length\_held\). If some of the balance is withdrawn, the remaining balance must be held longer.

## 8. Proposed ways of de-registering SON

An SON can be deregistered in two ways according to the functional requirements,

1. remove\_son called for SON in any state by the user from the wallet.
2. If an active SON is in maintenance for more than 12 hours.

## 9. Design changes suggested

### 9.1 son\_vesting\_amount

This is the minimum amount that should be vested in order to become an SON.

Currently, this is proposed to be 50 PPY.

This parameter can be changed through _**committee\_member\_update\_global\_parameters\_operation**_.

### 9.2 son\_vesting\_period

This is the minimum vesting duration a user has to wait from the time an SON is considered to be deregistered.

Currently, this is proposed to be 2 days \( 2 \* 86400 seconds\).

This parameter can be changed through _**committee\_member\_update\_global\_parameters\_operation**_.

### 9.3 vesting\_balance\_type

A new vesting\_balance\_type **son** is introduced like the one similar to **gpos**.

This helps in identifying the type of vesting amount.

### 9.4 dormant\_vesting\_policy&lt;linear\_vesting\_policy&gt;

This new policy just acts a wrapper around the existing policies.

As mentioned above the challenge in implementing the son vesting is that exact vesting duration is not known at the time of the creation of vesting\_balance\_object.

Till the point, an SON is considered as deregistered this policy would be in dormant mode and declines any vesting withdrawals initiated by the user.

Once an SON is marked as deregistered, the policy comes out of dormant mode and vesting duration is set to 2 days from deregistration.

From then on policy would just act as **linear\_vesting\_policy**.

### 9.5 update\_active\_sons

This function is introduced into the **database** class to handle the task of picking top N SONs at maintenance interval.

This also performs the task of deregistering the SON and activating the **new dormant policy** residing inside **son\_object**.

### 9.6 son\_status

This enum holds the state of an SON.

**active** - An SON user account selected among the top N SONs. \( N = 15 proposed currently \).

**inactive** - An SON who is idle and not actively performing the duties of maintaining the sidechain operations of another smart coin.

**maintenance** - An SON has gone down for maintenance but not deregistered yet.

### 9.7 create\_vesting\_balance

Argument is\_gpos needs to be changed to either a string or an enum to cater to the new requirements of son vesting balance type.

## 10. UML and Sequence Diagrams

## 11. Scenarios to consider

### 11.1 SON gone down into maintenance mode and user issues remove\_son

**remove\_son** takes precedence and the node is deregistered at the next maintenance interval.

The new dormant vesting policy is activated.

### 11.2 remove\_son followed by create\_vesting\_balance/create\_son within the 2-day vesting period

TODO: This should be specified in the FR Document.

### 11.3 SON gone down into maintenance mode, then de-registered after 12 hours period and user issues remove\_son

The first timestamp when the SON initially de-registered takes precedence.

