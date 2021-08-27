# user-sidechain-addresses-mapping

User sidechain address mapping is a component containing sidechain addresses belonging to a particular Peerplays user account. User sidechain addresses mapping is used in sidechain handler, a component of a SON plugin, for filtering events reported by listener, to identify the transactions of interest.

Transactions of interest are:

* For deposit handling, transfer on a sidechain network where from address is listed in user sidechain addresses mapping and to address is a sidechain address controlled by SON network \(Primary wallet\).
* For withdrawal handling, still to decide… E.g. transfer on a Peerplays network where from address is listed in user sidechain addresses mapping and to address is a address on a Peerplays network controlled by SON network \(Primary wallet\).

## Requirements

* In database terms, user sidechain address mapping is one-to-many relation, where one user can have multiple addresses belonging to him on a different blockchain. No duplicates are allowed, so user can register only one address per sidechain.
* User sidechain address mapping contains records with Peerplays account as a reference to user, and a string representing the address/account on a target sidechain.
* User addresses can be added, deleted and updated. Update and delete may be executed only by user to owning the record.
* User sidechain address mapping operations
  * add\_user\_sidechain\_address - adds user sidechain address
  * update\_user\_sidechain\_address - updates user sidechain address
  * delete\_user\_sidechain\_address - deletes user sidechain address
* User sidechain address mapping wallet functions
  * add\_user\_sidechain\_address - adds user sidechain address
  * update\_user\_sidechain\_address - updates user sidechain address
  * delete\_user\_sidechain\_address - deletes user sidechain address
  * get\_user\_sidechain\_address - gets a single user sidechain address \(by user account id and network\)
  * list\_user\_sidechain\_address - lists all users sidechain addresses
  * get\_sidechain\_addresses - gets a list of all addresses belonging to a given sidechain network

### Example of user sidechain addresses mapping

Following table shows user sidechain addresses mapping

| **User account ID** | **Network** | **Addreses** |
| :--- | :--- | :--- |
| 1.1.1 | network::bitcoin | BitcoinAddress1 |
| 1.1.1 | network::ethereum | EthereumAddress1 |
| 1.1.2 | network::bitcoin | BitcoinAddress2 |
| 1.1.3 | network::bitcoin | BitcoinAddress3 |
| 1.1.4 | network::ethereum | EthereumAddress4 |

## Implementation

Consider the following code as a suggestion, not as a 100% completed ad working implementation. Syntax errors may be present.

Following declaration needs to be moved from SON plugin, to the core:

enum networks {

bitcoin,

eos,

ethereum

};

User sidechain address object:

class user\_sidechain\_address\_object : public graphene::db::abstract\_object&lt;user\_sidechain\_addresses\_object&gt; {

uint32\_t id; // Primary key

account\_id\_type user; // User owning the address

network network; // Network to whom address belongs

std::string address; // Sidechain address

}

User sidechain address mapping index:

struct by\_network;

struct by\_user\_network;

using user\_sidechain\_address\_multi\_index\_type = multi\_index\_container&lt;

user\_sidechain\_address\_object,

indexed\_by&lt;

ordered\_unique&lt; tag&lt;by\_id&gt;,

member&lt;object, object\_id\_type, &object::id&gt;

&gt;,

ordered\_unique&lt; tag&lt;by\_network&gt;,

member&lt;user\_sidechain\_address\_object, network, &user\_sidechain\_address\_object::network&gt;

&gt;,

ordered\_unique&lt; tag&lt;by\_user\_network&gt;,

composite\_key&lt;user\_sidechain\_address\_object,

member&lt;user\_sidechain\_address\_object, account\_id\_type, &user\_sidechain\_address\_object::user&gt;,

member&lt;user\_sidechain\_address\_object, network, &user\_sidechain\_address\_object::network&gt;,

&gt;

&gt;

&gt;

&gt;;

* User sidechain address mapping should be indexed at least by:
  * Primary key index
  * Network field
  * User and network fields

User sidechain address create operations:

struct user\_sidechain\_address\_create\_operation : public base\_operation

{

uint32\_t id; // Primary key

account\_id\_type user; // User owning the address

network network; // Network to whom address belongs

std::string address; // Sidechain address

void validate\(\)const;

};

Validation should check that the item is not duplicated, by checking user and network fields.

User sidechain address update operations:

struct user\_sidechain\_address\_update\_operation : public base\_operation

{

uint32\_t id; // Primary key

account\_id\_type user; // User owning the address

network network; // Network to whom address belongs

std::string address; // Sidechain address

void validate\(\)const;

};

Validation should check that the item with the given id exists in the list.

User sidechain address delete operation:

struct user\_sidechain\_address\_delete\_operation : public base\_operation

{

uint32\_t id; // Primary key

void validate\(\)const;

};

Validation should check that the item with the given id exists in the list.

Appropriate evaluators must be implemented too.

