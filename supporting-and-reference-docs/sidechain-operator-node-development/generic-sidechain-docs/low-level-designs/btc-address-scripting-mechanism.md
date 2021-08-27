# btc-address-scripting-mechanism

## Purpose

This doc describes the required changes to Bitcoin multisignature wallet scripting mechanism \(previously described in Multisignature wallet LLD\).

## Scope

TODO

## Process Overview

SONs are expected to be co-signatories for the funds stored on PW. Each SON has a public key on Bitcoin \(secp256k1\) and the weight of its signature.

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

The point of the script is that each SON has a weight \(sum of votes for SON that is set at maintenance\). Depending on the cumulative weight of votes a decision is made on whether the transfer is to be executed. For the transfer to happen the sum of weights in the script must be &gt;= 2/3 + 1 from the number of current active SON weights.

Since the addresses depend on weights, and weights in turn depend on balances, on most occasions the deposit addresses will be re-generated after maintenance. There are two ways to handle this situation:

1. All pre-maintenance are to be considered invalid after maintenance.
2. Consider the pre-maintenance composition of SONs and their weights trusted post-maintenance. If the composition of SONs doesn't change, but their weights do, SON members are to be allowed to process the transaction in accordance with their pre-maintenance weights.

We can change the ranking upon the first transaction related to PW/SONs takes place. This is further discussed here : [Functional Specification - SONs switchover scenarios\#SONsswitchoverscenarios-6.Scenarios](https://peerplays.atlassian.net/wiki/spaces/PIX/pages/307003405/Functional+Specification+-+SONs+switchover+scenarios#FunctionalSpecification-SONsswitchoverscenarios-SONsswitchoverscenarios-6.Scenarios)

