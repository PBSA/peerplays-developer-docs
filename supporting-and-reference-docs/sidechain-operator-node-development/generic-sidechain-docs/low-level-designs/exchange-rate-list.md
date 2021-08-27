# exchange-rate-list

Exchange rate list is a component containing exchange rates between 1 unit of target sidechain native currency and pBTC. Exchange rates are used during deposit and withdrawal process, for calculating how much pBTC should be issued for a given amount of sidechain native currency, or how much sidechain native currency should be paid to the user for a given amount of pBTC.

Exchange rate list operations should be implemented in core, not in a plugin. **Using exchange rate list operations should not be allowed to all users.** E.g. only committee members or Peerplays network administrators may use them. **However, handling exchange rates as proposals, and waiting for network members to vote, may not be the best solution**, as the circumstances might require that exchange rate becomes active immediately, to prevent network misuse, or pulling funds on unfair conditions \(e.g. when currency value variations are too high, or a lot of users start pulling funds out of the network\).

Access to functions to add, update or delete exchange rate, should not be allowed to all users on the network, but only to users which are considered Peerplays network adminsitrators, or committee members.

## Requirements

* Exchange rate list contains exchange rates between 1 unit of target sidechain native currency and pBTC.
* All values are stored as int64\_t \(or uint\_64\_t, still to decide\), and expressed in minimal currency units \(1 BTC = 100000000 Satoshi’s, 1 ETH = 1000000000000000000 wei’s, 1 EOS = 1000 whatever, etc…\)
* Exchange rate can be added, deleted and updated by authorized entity \(user or SON network\)
* Exchange rate list can be expanded with timestamp field, showing UTC date and time from when the exchange rate should be used. Version with timestamp is harder to implement and maintain.
* Exchange rate list operations
  * create\_exchange\_rate - creates exchange rate
  * update\_exchange\_rate - updates exchange rate
  * delete\_exchange\_rate - deletes exchange rate
* Exchange rate wallet functions
  * create\_exchange\_rate - creates exchange rate
  * update\_exchange\_rate - updates exchange rate
  * delete\_exchange\_rate - deletes exchange rate
  * get\_exchange\_rate - gets a single exchange rate \(by id, or network, or network and timestamp\)
  * list\_exchange\_rates - lists all exchange rates

### Examples of exchange rate list

Following table shows simplest exchange rate list with no saved history of exchange rates

| **Currency** | **Currency amount** | **pBTC amount** |
| :--- | :--- | :--- |
| BTC | 100000000 | X |
| EOS | 1000 | Y |
| ETH | 1000000000000000000 | Z |

Following table shows exchange rate list, expanded with timestamp field, which is able to save exchange rate history

|  |  |  |  |
| :--- | :--- | :--- | :--- |
| BTC | 2019-11-20 00:00:00 | 100000000 | X |
| BTC | 2019-11-25 00:00:00 | 100000000 | X1 |
| EOS | 2019-11-20 10:00:00 | 1000 | Y |
| EOS | 2019-11-24 14:00:00 | 1000 | Y1 |
| ETH | 2019-11-21 07:00:00 | 1000000000000000000 | Z |
| ETH | 2019-11-23 00:00:00 | 1000000000000000000 | Z1 |

## Implementation

Consider the following code as a suggestion, not as a 100% completed ad working implementation. Syntax errors may be present.

Following declaration needs to be moved from SON plugin, to the core:

enum networks {

bitcoin,

eos,

ethereum

};

Exchange rate object:

class exchange\_rate\_object : public graphene::db::abstract\_object&lt;exchange\_rate\_object&gt; {

uint32\_t exchange\_rate\_id; // Primary key

network network; // Network managing the currency

int64\_t exchange\_from; // Network native currency amount

int64\_t exchange\_to; // Peerplays asset amount \(pBTC\)

}

// Version with timestamp

class exchange\_rate\_object : public graphene::db::abstract\_object&lt;exchange\_rate\_object&gt; {

uint32\_t exchange\_rate\_id; // Primary key

network network; // Network managing the currency

time\_point\_sec valid\_from; // Starting date when exchange rate becomes valid

int64\_t exchange\_from; // Network native currency amount

int64\_t exchange\_to; // Peerplays asset amount \(pBTC\)

}

Exchange rate list index:

struct by\_network;

using exchange\_rate\_multi\_index\_type = multi\_index\_container&lt;

exchange\_rate\_object,

indexed\_by&lt;

ordered\_unique&lt; tag&lt;by\_id&gt;,

member&lt;object, object\_id\_type, &object::id&gt;

&gt;,

ordered\_unique&lt; tag&lt;by\_network&gt;,

member&lt;exchange\_rate\_object, network, &exchange\_rate\_object::network&gt;

&gt;

&gt;

&gt;;

// Version with timestamp

struct by\_network\_valid\_from;

using exchange\_rate\_multi\_index\_type = multi\_index\_container&lt;

exchange\_rate\_object,

indexed\_by&lt;

ordered\_unique&lt; tag&lt;by\_id&gt;,

member&lt;object, object\_id\_type, &object::id&gt;

&gt;,

ordered\_unique&lt; tag&lt;by\_network\_valid\_from&gt;,

composite\_key&lt;exchange\_rate\_object,

member&lt;exchange\_rate\_object, network, &exchange\_rate\_object::network&gt;,

member&lt;exchange\_rate\_object, time\_point\_sec, &exchange\_rate\_object::valid\_from&gt;,

&gt;

&gt;

&gt;

&gt;;

* Exchange rate list should be indexed at least by network and valid\_from fields, so a composite key is required.
* Exchange rate list will be queried with parameters network and timestamp, and the query should return exchange rate valid at a date and time from the timestamp. Exchange rate is valid from the latest timestamp less than the timestamp parameter. E.g. we have the following exchange rate list:

  BTC, 2019-11-20 00:00:00, 100000000, X  
  BTC, 2019-11-25 00:00:00, 100000000, X1

  If we query the index with \(network::bitcoin, 2019-11-23\), query should return value X, since that exchange rate is valid in the period of \[2019-11-20 00:00:00 to 2019-11-24 23:59:59\]

  Read Boost MultiIndex docs, to learn about all the ways this is possible to do:  
  [https://www.boost.org/doc/libs/1\_67\_0/libs/multi\_index/doc/tutorial/basics.html\#special\_lookup](https://www.boost.org/doc/libs/1_67_0/libs/multi_index/doc/tutorial/basics.html#special_lookup)

Exchange rate create operations:

struct exchange\_rate\_create\_operation : public base\_operation

{

uint32\_t exchange\_rate\_id; // Primary key

network network; // Network managing the currency

int64\_t exchange\_from; // Network native currency amount

int64\_t exchange\_to; // Peerplays asset amount \(pBTC\)

void validate\(\)const;

};

// Version with timestamp

struct exchange\_rate\_create\_operation : public base\_operation

{

uint32\_t exchange\_rate\_id; // Primary key

network network; // Network managing the currency

time\_point\_sec valid\_from; // Starting date when exchange rate becomes valid

int64\_t exchange\_from; // Network native currency amount

int64\_t exchange\_to; // Peerplays asset amount \(pBTC\)

void validate\(\)const;

};

Validation should check that the item is not duplicated. Eventually, it can check exchange\_from precision, since they are known in advance.

Exchange rate update operations:

struct exchange\_rate\_update\_operation : public base\_operation

{

uint32\_t exchange\_rate\_id; // Primary key

network network; // Network managing the currency

int64\_t exchange\_from; // Network native currency amount

int64\_t exchange\_to; // Peerplays asset amount \(pBTC\)

void validate\(\)const;

};

// Version with timestamp

struct exchange\_rate\_update\_operation : public base\_operation

{

uint32\_t exchange\_rate\_id; // Primary key

network network; // Network managing the currency

time\_point\_sec valid\_from; // Starting date when exchange rate becomes valid

int64\_t exchange\_from; // Network native currency amount

int64\_t exchange\_to; // Peerplays asset amount \(pBTC\)

void validate\(\)const;

};

Validation should check that the item with the given exchange\_rate\_id exists in the list.

Exchange rate list delete operation:

// Same for both versions

struct exchange\_rate\_delete\_operation : public base\_operation

{

uint32\_t exchange\_rate\_id; // Primary key

void validate\(\)const;

};

Validation should check that the item with the given exchange\_rate\_id exists in the list.

Appropriate evaluators must be implemented too.

