# wallet-commands-for-son

## 1. Purpose

The purpose of this document is to provide the design for the commands to be added to the CLI Wallet for SONs.

## 2. Scope

The design requirement listed in this document will be limited to the commands to be added to the CLI Wallet. This will outline the commands required to be added and their implementation. Also outlined here will be the sequence in which the required steps will be performed including:

* any interactions with the user.
* validations to ensure complete and accurate information gathering.

## 3. Background

The Bitcoin Sidechain functionality has been implemented in the Peerplays blockchain but it doesn't take into account, the change of witnesses. As per the current implementation of Sidechain, a multisig bitcoin address will be created on the bitcoin blockchain to hold the bitcoins that have been deposited into the pBTC accounts of the Peerplays users. Every Peerplays witnesses will have a bitcoin transaction signing key for this multisig bitcoin address and will be required to sign any withdrawal transaction. When a witness changes, the transaction signing key of the outgoing witness needs to be removed from the multisig bitcoin address and the key of the incoming witness needs to be added. The suggested proposal is to make the Sidechain code available as a plugin and assign the responsibility for running the sidechain code to separate nodes called the Sidechain Operating Nodes \(SONs\). The SON functionality will be independent of the witness functionality and SONs don't need to be changed much often.

## 4. Process Overview

Described here is the design for commands to be added to the CLI Wallet for adding/removing SON, voting for SON, etc.

## 5. Context

The following commands will have to be added in the CLI wallet and new APIs created for the same to allow the users to use the SON functionality:

a. create\_son\(string owner\_account, string url, bool broadcast\)  
This command creates a transaction to create a new SON. The parameters required are:  
owner\_account: account name which has to be registered as an SON,  
url: published url where the SON has published his details and his marketing pitch and  
broadcast: whether to broadcast this transaction or not \(default: false\).

b. vote\_for\_son\(string voting\_account, string son\_account, bool approve, bool broadcast\)  
This command creates a transaction to vote for an SON. The parameters required are:  
voting\_account: account name which is voting  
son\_account: account name of the SON to whom the votes are being cast  
approve: whether the votes are being cast or withdrawn \(default: false\)  
broadcast: whether to broadcast this transaction or not \(default: false\)

c. get\_son\(string owner\_account\)  
Command to get the details of an SON. The parameters required are:  
owner\_account: account name of the SON

d. update\_son\(string son\_name, string url, string bitcoin\_signing\_key, bool broadcast\)  
This command creates a transaction to update the SON. The parameters required are:  
son\_name: The name of the SON to be updated  
url: published url where the SON has published his details and his marketing pitch to be updated  
bitcoin\_signing\_key: bitcoin transaction signing key of the SON to be updated  
broadcast: whether to broadcast this transaction or not \(default: false\)

e. update\_son\_votes\(string voting\_account, std::vector&lt;std::string&gt; sons\_to\_approve, std::vector&lt;std::string&gt; sons\_to\_reject, uint16\_t desired\_number\_of\_sons, bool broadcast\)  
This command creates a transaction to update the votes for SONs. The parameters required are:  
voting\_account: The account name which is voting  
sons\_to\_approve: The SONs to which the votes have to be casted  
sons\_to\_reject: The SONs from which the votes have to be removed  
desired\_number\_of\_sons: The total number of SONs that the user wants to vote for  
broadcast: whether to broadcast this transaction or not \(default: false\)

f. list\_sons\(string& lowerbound, uint32 limit\)  
Command to list the SONs starting from a lowerbound account id.  
lowerbound: The account id to start searching SONs from  
limit: The max number of SONs to be returned

g. claim\_registered\_son\(const std::string& son\_name\)  
Command to save the private keys of the registered SON in the wallet permanently after the registration of SON is successful. The SON should have been created from the same CLI wallet.  
son\_name: The name of the SON whose private keys are being saved in the wallet.

h. remove\_son\(string owner\_account, bool broadcast\)  
This command creates the transaction for remove\_son\_operation. The parameters required are:  
owner\_account: The account name of the witness to be removed.  
broadcast: whether to broadcast this transaction or not \(default: false\)

i. list\_active\_sons\(string& lowerbound, uint32 limit\)

Command to list the SONs that are active currently.

We can use global\_property\_object::active\_sons to retrieve the currently active SONs list.

lowerbound: The account id to start searching SONs from  
limit: The max number of SONs to be returned

The following commands will have to be updated in the CLI wallet and in the existing APIs to add the functionality related to SONs:

a. get\_global\_properties\(\)  
This command returns the global properties of the blockchain such as head\_block\_num, head\_block\_id, next\_maintenance\_time, active\_witnesses, active\_committee\_members, etc. This should be modified to provide the active\_sons as well.

b. resync\(\) method  
The resync method in the wallet.cpp file updates the wallet\_data annotations e.g. wallet has been restarted and was not notified of the events while it was down.

