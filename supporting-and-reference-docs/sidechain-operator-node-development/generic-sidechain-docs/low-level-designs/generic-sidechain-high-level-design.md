# generic-sidechain-high-level-design

## Objective

This document is intended to outline generic design of sidechain plugin used by Peerplays Node, to monitor and process events of interest on a target sidechain nodes. This interface will be extended to meet various other integrations.

## Assumptions

* Sidechain plugin purpose is to monitor and process events of interest on a multiple target sidechains.
* Sidechain node provides interface for monitoring changes in a sidechain.
* Monitoring is based on new block, new transaction or filtered single event (like transfer operation to specific address).
* There is a sufficient C/C++ library for connecting to monitoring interface.
* Sidechain node provides communication interface which sidechain handler can use to read all the required info about event of interest. E.g. HTTP/RPC…

## Block Diagram

Link to [Draw.io](http://draw.io/) file

[https://drive.google.com/file/d/1BXeRwK\_2PNt6wMnzIl8M6OMRnuM5CumQ/view?usp=sharing](https://drive.google.com/file/d/1BXeRwK\_2PNt6wMnzIl8M6OMRnuM5CumQ/view?usp=sharing)

## Description

* Peerplays Sidechain is implemented as a plugin.
* Sidechain Manager is a component in a Peerplays Plugin, which contains a collection of Sidechain Handlers.
* No multiple sidechain handlers for same network is allowed.
* No communication between sidechain handlers is allowed or needed.
* One Sidechain Handler handles only one sidechain network.
* Each Sidechain Handler monitors and processes events of interests on its target network.
* Each Sidechain Handler implements only sidechain specific processing, eg reading transaction info, parsing transaction data, etc…
* The result of Sidechain Handler processing saved into peerplays network for further processing
* In general, Sidechain Handler contains event listener, communication interface and event handler.
* Event listener is “listening” for events on a sidechain. When event happens, event listener will notify event handler about incoming event.
* Event handler uses communication interface to read all the information needed in order to process the event.
* Once the processing is finished, event handler will pass the result to the Sidechan Manager, for further processing.

## Sequence diagram

Link to [draw.io](http://draw.io/) file

[https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing](https://drive.google.com/file/d/17kbez7C1Djaj-2AgyEzQ1Z-5CrSZ5wZE/view?usp=sharing)

For the simplicity, sequence diagram shows interactions between components in a single sidechain handler.

## Suggested implementation

* **peerplays\_sidechain\_plugin** is a class implementing a peerplays sidechain plugin.
  * Contains the instance of **sidechain\_manager**
* **sidechain\_manager** is a class implementing Sidechain Manager
  * Contains the collection (std::vector) of **sidechain\_handlers**
  * Contains the factory method for creating instance of **sidechain\_handler** for particular sidechain
  * Factory method parameters include program options, used to pass configure options to a sidechain handler
  * Contains methods for calling sidechain\_handlers methods for processing events of interest
  * **recreate\_primary\_wallet** This method will initiate recreation of primary wallet on active SON set change, if needed
  * **process\_deposits** This method will call sidechain handlers method for processing sidechain deposits
  * **process\_withdrawals** This method will call sidechain handlers method for processing sidechain withdrawals
* **sidechain\_handler** is a base class for implementing sidechain handlers
  * Needs to have access to user sidechain address mapping list, in order to filter events by checking addresses involved in a transaction against the list of addresses of interests.
    * sidechain\_type get\_sidechain() Gets the sidechain type
    * std::vectorstd::string get\_sidechain\_deposit\_addresses() Gets the list of deposit addresses for a sidechain
    * std::vectorstd::string get\_sidechain\_withdraw\_addresses() Gets the list of withdrawal addresses for a sidechain
    * std::string get\_private\_key(std::string public\_key) Gets the private key for a given public key, for signing sidechain transactions
  * Needs to contain method receiving the data structure describing event of interest as the parameter
    * sidechain\_event\_data\_received(const sidechain\_event\_data \&sed)
    * // Sidechain type
    * enum class sidechain\_type {
    * bitcoin,
    * ethereum,
    * eos,
    * peerplays
    * };
    * struct sidechain\_event\_data {
    * fc::time\_point\_sec timestamp; // time when event was detected
    * sidechain\_type sidechain; // sidechain where event happened
    * std::string sidechain\_uid; // transaction unique id for a sidechain
    * std::string sidechain\_transaction\_id; // transaction id on a sidechain
    * std::string sidechain\_from; // sidechain senders account, if available
    * std::string sidechain\_to; // sidechain receiver account, if available
    * std::string sidechain\_currency; // sidechain transfer currency
    * fc::safe\<int64\_t> sidechain\_amount; // sidechain asset amount
    * chain::account\_id\_type peerplays\_from; // perplays senders account matching sidechain senders account
    * chain::account\_id\_type peerplays\_to; // peerplays receiver account matching sidechain receiver account
    * chain::asset peerplays\_asset; // transfer value in peerplays core asset

};

*

    * For deposits, sidechain handler will create deposit descriptor object **son\_wallet\_deposit\_object**, using data from sidechain\_evend\_data structure
    * class son\_wallet\_deposit\_object : public abstract\_object\<son\_wallet\_deposit\_object>
    * {
    * public:
    * static const uint8\_t space\_id = protocol\_ids;
    * static const uint8\_t type\_id = son\_wallet\_deposit\_object\_type;
    * time\_point\_sec timestamp;
    * peerplays\_sidechain::sidechain\_type sidechain;
    * int64\_t confirmations;
    * std::string sidechain\_uid;
    * std::string sidechain\_transaction\_id;
    * std::string sidechain\_from;
    * std::string sidechain\_to;
    * std::string sidechain\_currency;
    * safe\<int64\_t> sidechain\_amount;
    * chain::account\_id\_type peerplays\_from;
    * chain::account\_id\_type peerplays\_to;
    * chain::asset peerplays\_asset;
    * bool processed;

    };
*

    * For withdrawals, sidechain handler will create withdrawal descriptor object **son\_wallet\_withdraw\_object**, using data from sidechain\_event\_data structure
    * class son\_wallet\_withdraw\_object : public abstract\_object\<son\_wallet\_withdraw\_object>
    * {
    * public:
    * static const uint8\_t space\_id = protocol\_ids;
    * static const uint8\_t type\_id = son\_wallet\_withdraw\_object\_type;
    * time\_point\_sec timestamp;
    * peerplays\_sidechain::sidechain\_type sidechain;
    * int64\_t confirmations;
    * std::string peerplays\_uid;
    * std::string peerplays\_transaction\_id;
    * chain::account\_id\_type peerplays\_from;
    * chain::asset peerplays\_asset;
    * peerplays\_sidechain::sidechain\_type withdraw\_sidechain;
    * std::string withdraw\_address;
    * std::string withdraw\_currency;
    * safe\<int64\_t> withdraw\_amount;
    * bool processed;

    };
*
  * Contains following abstract methods (the list can change anytime):
    * **virtual void recreate\_primary\_wallet() = 0;** Method is called by sidechain manager, to recreate the primary wallet on a sidechain, if needed
    * **virtual void process\_deposit(const son\_wallet\_deposit\_object \&swdo) = 0;** Callback method, called for each deposit that needs to be processed by a sidechain handler
    * **virtual void process\_withdrawal(const son\_wallet\_withdraw\_object \&swwo) = 0;**
    * Callback method, called for each withdrawal that needs to be processed by a sidechain handler
*   **sidechain\_handler\_bitcoin** is a class, inheriting **sidechain\_handler**, implementing sidechain handler for Bitcoin

    * Listener may be implemented by ZeroMQ (also spelled ØMQ, 0MQ or ZMQ), high-performance asynchronous messaging library
    * Communication interface may be implemented as HTTP client, using RPC bitcoin node interface
      * Implement any RPC call that might be needed
      * std::string addmultisigaddress(const std::vector\<std::string> public\_keys);
      * std::string createrawtransaction(const std::vector\<btc\_txout> \&ins, const fc::flat\_map\<std::string, double> outs);
      * std::string createwallet(const std::string \&wallet\_name);
      * std::string encryptwallet(const std::string \&passphrase);
      * uint64\_t estimatesmartfee();
      * std::string getblock(const std::string \&block\_hash, int32\_t verbosity = 2);
      * void importaddress(const std::string \&address\_or\_script);
      * std::vector\<btc\_txout> listunspent();
      * std::vector\<btc\_txout> listunspent\_by\_address\_and\_amount(const std::string \&address, double transfer\_amount);
      * std::string loadwallet(const std::string \&filename);
      * void sendrawtransaction(const std::string \&tx\_hex);
      * std::string signrawtransactionwithkey(const std::string \&tx\_hash, const std::string \&private\_key);
      * std::string signrawtransactionwithwallet(const std::string \&tx\_hash);
      * std::string unloadwallet(const std::string \&filename);

    std::string walletlock();
*
  * Implements abstract methods:
    * **void recreate\_primary\_wallet() override;** Method is called by sidechain manager, to recreate the primary wallet on a sidechain, if needed
    * **void process\_deposit(const son\_wallet\_deposit\_object \&swdo) override;** Callback method, called for each deposit that needs to be processed by a sidechain handler
    * **void process\_withdrawal(const son\_wallet\_withdraw\_object \&swwo) override;** Callback method, called for each withdrawal that needs to be processed by a sidechain handler
* **sidechain\_handler\_ethereum** is a class, inheriting **sidechain\_handler**, implementing sidechain handler for Ethereum
  * TBD
* **sidechain\_handler\_eos** is a class, inheriting **sidechain\_handler**, implementing sidechain handler for EOS
  * TBD
* **sidechain\_handler\_peerplays** is a class, inheriting **sidechain\_handler**, implementing sidechain handler for Peerplays
  * Listener can be implemented as a callback of database.applied\_block signal. This will give us access to newly created blocks, and its content.
  * Communication interface, like RPC client from Bitcoin handler, is not really needed, as we can read all the data we need from blockchain database.
  * Implements abstract methods:
    * **void recreate\_primary\_wallet() override;** Method is called by sidechain manager, to recreate the primary wallet on a sidechain, if needed
    * **void process\_deposit(const son\_wallet\_deposit\_object \&swdo) override;** Callback method, called for each deposit that needs to be processed by a sidechain handler
    * **void process\_withdrawal(const son\_wallet\_withdraw\_object \&swwo) override;** Callback method, called for each withdrawal that needs to be processed by a sidechain handler
* **sidechain\_handler\_\*** is a class, inheriting **sidechain\_handler**, implementing sidechain handler for \*
  * Any future sidechain
  * TBD
