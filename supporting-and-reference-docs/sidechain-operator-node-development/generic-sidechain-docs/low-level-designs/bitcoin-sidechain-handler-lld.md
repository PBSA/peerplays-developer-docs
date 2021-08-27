# bitcoin-sidechain-handler-lld

## Objective

This document is intended to outline generic design of a blockchain handler, a component used for monitoring and processing changes on a Bitcoin network.

For general information on sidechain handlers, read:  
[https://peerplays.atlassian.net/wiki/spaces/PIX/pages/352026689/Generic+Sidechain+Interface+High+Level+Design](https://peerplays.atlassian.net/wiki/spaces/PIX/pages/352026689/Generic+Sidechain+Interface+High+Level+Design)

## Block Diagram

Link to [draw.io](http://draw.io/) file:

[https://drive.google.com/file/d/1ErmQfeaWa9m67si4hb0RZs54WVYWR0pC/view?usp=sharing](https://drive.google.com/file/d/1ErmQfeaWa9m67si4hb0RZs54WVYWR0pC/view?usp=sharing)

## Sequence diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing](https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing)

Same sequence diagram applies for all sidechain handlers. It shows interactions between components in a single sidechain handler.

## Components

The purpose of Bitcoin sidechain handler is to notify sidechain manager that a transaction of interest has happened, and forward the data about that transaction to it, for further processing. As stated in general design, standard components of the sidechain handler are listener, communication interface and event handler.

### Listener

For general information on sidechain listeners, read:  
[https://peerplays.atlassian.net/wiki/spaces/PIX/pages/352092181/Generic+Sidechain+Listener+HLD](https://peerplays.atlassian.net/wiki/spaces/PIX/pages/352092181/Generic+Sidechain+Listener+HLD)

Some of the info in this section are taken from useful article on ZMQ and Bitcoin [https://bitcoindev.network/accessing-bitcoins-zeromq-interface/](https://bitcoindev.network/accessing-bitcoins-zeromq-interface/)

#### Assumptions

**Blockchain node provides interface for monitoring changes in a blockchain.**

Bitcoin node provides ZeroMQ messaging interface. The ZeroMQ facility implements a notification interface through a set of specific notifiers. Currently there are notifiers that publish blocks and transactions. This read-only facility requires only the connection of a corresponding ZeroMQ subscriber port in receiving software; it is not authenticated nor is there any two-way protocol involvement. Therefore, subscribers should validate the received data since it may be out of date, incomplete or even invalid.

**Monitoring is based on new block, new transaction or filtered single event \(like transfer operation to specific address\)**

Bitcoin's ZMQ uses a publish/subscribe \(pubsub\) design pattern, where the publisher in our case is our bitcoin node which published messages based on events. The subscriber on the other hand, includes any application looking to utilise it by subscribing to these events. There are currently 4 different publish \(PUB\) notification topics we can expose via bitcoind which notify us when ever a new block or transaction is validated by the node.

* zmqpubhashtx : Publishes transaction hashes
* zmqpubhashblock : Publishes block hashes
* zmqpubrawblock : Publishes raw block information
* zmqpubrawtx : Publishes raw transaction information

We can use any of these, but some are more practical than others. E.g. we can subscribe to receive hash values of newly created blocks:

socket.setsockopt\( ZMQ\_SUBSCRIBE, "hashblock", 0 \);

socket.connect\( "tcp://" + ip + ":" + std::to\_string\( zmq\_port \) \);

zmqpubrawtx looks like best candidate we can use, since we can get raw transaction info, decode it, and we will have all required info in a single call. However, depending on number of transaction, we can expect more traffic between bitcoin node and listener.

zmqpubhashblock is also a good candidate, we will receive the hash of newly created block, then we can read the whole block by communication interface, parse it and get the info we need.

**There is a sufficient C/C++ library for connecting to monitoring interface**

ZeroMQ project provides a client library which we can use to connect to a Bitcoin node ZeroMQ interface. The project is alive and well maintained. At the time of writing this document, we are not aware of any problem which would require us to use specific library version, or link it statically into our software.

[https://zeromq.org/](https://zeromq.org/)

[https://github.com/zeromq/libzmq](https://github.com/zeromq/libzmq)

There is a libzmq3-dev package in Ubuntu 18.04 repository.

[https://packages.ubuntu.com/bionic/libzmq3-dev](https://packages.ubuntu.com/bionic/libzmq3-dev)

This will be obvious developers choice, for ease of installation and usage.

### Communication interface

Bitcoin provides RPC interface we can use to read additional transaction info from a node.

Depending on how we configure the listener, we will need more or less functionalities in this component.

In general, we will need:

* Method to retrieve the whole block by hash
  * [https://bitcoin.org/en/developer-reference\#getblock](https://bitcoin.org/en/developer-reference#getblock)
* Method to retrieve the transaction by hash
  * [https://bitcoin.org/en/developer-reference\#gettransaction](https://bitcoin.org/en/developer-reference#gettransaction)
* Method to retrieve block as json
  * HTTP request to "[http://IP:PORT/rest/block/BLOCK\_HASH.json](http://IP:PORT/rest/block/BLOCK_HASH.json)";
* Method to decode transaction
  * [https://bitcoin.org/en/developer-reference\#decoderawtransaction](https://bitcoin.org/en/developer-reference#decoderawtransaction)
* Method to retrieve the transaction fee
  * [https://bitcoin.org/en/developer-reference\#estimatesmartfee](https://bitcoin.org/en/developer-reference#estimatesmartfee)
* Method to send signed transaction to the node \(for deposit/withdrawal\)
  * [https://bitcoin.org/en/developer-reference\#sendrawtransaction](https://bitcoin.org/en/developer-reference#sendrawtransaction)

Communication interface can be implemented as a HTTP client, which will send requests to a bitcoin node.

### Event Handler

Event handler is a working horse of sidechain handler. It will receive input from a listener, and process it any way needed, in order to get the minimum required information about event of interest, namely transactions to the addresses we are interested in.

In general, event handler needs:

* Access to the list of addresses we are interested in

## Suggested implementation

* **sidechain\_manager** is a class implementing Sidechain Manager
  * Introduce a new element of network enum, to indicate that we want to use bitcoin network
  * namespace graphene { namespace peerplays\_sidechain {
  * enum networks {
  * bitcoin
  * ...
  * };
  * class sidechain\_net\_manager {

...

* ...
  ...

* Implement **sidechain\_handler\_bitcoin** class, inheriting **sidechain\_handler**, implementing sidechain handler for Bitcoin
* Implement **zmq\_listener**, using ZeroMQ library

  * subsribe to hashblock or raw transaction
  * socket.setsockopt\( ZMQ\_SUBSCRIBE, "hashblock", 0 \);
  * socket.connect\( "tcp://" + ip + ":" + std::to\_string\( zmq\_port \) \);
  * * or
  * * socket.setsockopt\( ZMQ\_SUBSCRIBE, "rawtx", 0 \);

  socket.connect\( "tcp://" + ip + ":" + std::to\_string\( zmq\_port \) \);

* Implement **bitcoin\_rpc\_client** class, HTTP client used to communicate with Bitcoin node
  * Implement, at least, following methods:
    * **receive\_full\_block** - to retrieve full block by block hash
    * **receive\_transaction** - to retrieve transaction info by transaction hash
    * **receive\_block\_as\_json** - to retrieve full block as json by block hash
    * **decode\_transaction** - to decode raw bitcoin trasnaction
    * **receive\_estimated\_fee** - to retrieve transaction cost \(transaction fee\)
    * **send\_btc\_tx** - to send signed transaction to the bitcon node
* Implement any additional code needed, for receiving event data, collecting any additional information about event of interest, with the final result as a data structure containing the following info:
* struct sidechain\_transaction\_info {
* graphene::peerplays\_sidechain::networks network; // set to network::bitcoin
* std::string source;
* std::string destination;
* uint64\_t amount;
* uint64\_t transaction\_fee;

}

* * Identifier of a sidechain handler \(network::bitcoin\)
  * Transaction’s source address \(as string, representing sidechain native address\)
  * Transaction’s destination address \(as string, representing sidechain native address\)
  * Transaction’s amount \(as unsigned integer, in sidechain native currency\)
  * Estimated transaction fee \(as unsigned integer, in sidechain native currency\)

