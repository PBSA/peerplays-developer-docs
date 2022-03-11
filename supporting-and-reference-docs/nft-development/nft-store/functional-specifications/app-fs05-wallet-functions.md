---
description: >-
  The Peerplays NFT Store application functional requirements specification for
  the wallet functions.
---

# APP-FS05 Wallet Functions

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NFT Store application (the “app”) relating to the wallet functions from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NFT Store [Requirements Specification](https://devs.peerplays.tech/supporting-and-reference-docs/nft-development/nft-store/nft-store-requirements-specification).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`Page Fragment`

`Process`

## 3. Scope

This FS will describe the requirements and basic design for the app’s wallet functions. All valid economic models that can be configured will be detailed in this FS.

### 3.1. Components

Specific configurations, components, and processes covered in this FS include:

* configurable economic models
  * crypto only
  * crypto to market token
  * fiat to market token
  * hybrid crypto and fiat to market token
* wallet function processes
  * crypto functions
  * fiat functions
* wallet function components
  * wallet page fragment in header
    * wallet status
    * quick actions
  * wallet page fragment in profile page
    * list of user assets

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                                      |
| ---------- | -------------------------------------------- |
| APP-FS05-# | App Component Requirement - Wallet Functions |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

The following terms are used to describe specific users of the application:

* Unauthenticated (not logged in) users are known as `visitors`.
* Authenticated (logged in) users are known as `peers`.

The following terms are used to describe levels of user entitlement within the application:

* A `browser` is view only (except for account creation and logging in) and used for visitors.
* An `enjoyer` can interact with the market, including buying and optionally re-selling NFTs, but can’t make new NFTs.
* A `tenant` can create NFTs and sell them in addition to what the enjoyer can do.
* A `client` is an administrator level user with all entitlements.

## 5. Context

The wallet, in the context of the NFT Store application, handles the crypto and fiat functions that make the marketplace possible. It’s both a container for coins and tokens on the Peerplays chain, and a middleware to the gateways of fiat payment processing. Clients must configure the wallet functions while standing up an NFT Store instance and are responsible for daily operations regarding user wallet security and administration. Tenants will receive revenue on the sales of their NFTs to their wallet. Enjoyers use their wallet balances to bid on and purchase NFTs from tenants.

Depending on the deployment architecture, fiat may be used to buy special marketplace tokens which are used to drive the market economy. These tokens are only useful within the market for which they are created. The tokens can then be sold back to the issuer for fiat when a holder wishes to “cash out” and exit the market. All valid economic models that can be configured will be detailed in this FS.

## 6. Configurations

### 6.1. Crypto Only

This is the simplest configuration which allows peers to buy NFTs using their crypto coins or tokens. A peer can send any asset supported by the Peerplays chain to their wallet to use in the NFT Store. Currently this includes `PPY`, `BTC`, `HIVE`, and `HBD`. The following diagram illustrates a crypto only economy configuration.

![](<../../../../.gitbook/assets/APP-FS05-wallet-functions-Crypto Only.drawio.png>)

Figure 1. A crypto only economy configuration.

### 6.2. Crypto Market Token

The next step in complexity is to add an abstraction token to the economic model. In the crypto market token economy, a `market token` is used for all NFT buying and selling activities. This serves to reduce friction between buyers and sellers by eliminating the need to negotiate multiple currencies. In this case, value will be well established and peers will be well informed to make comparisons for buying decisions. This configuration requires the NFT Store to buy and sell the market token so that peers and tenants can enter and leave the market with crypto. The market token has no use or value outside of the NFT Store instance for which it’s made. The following diagram illustrates a crypto market token economy configuration.

![](<../../../../.gitbook/assets/APP-FS05-wallet-functions-Crypto Market Token.drawio.png>)

Figure 2. A crypto market token economy configuration.

### 6.3. Fiat Market Token

Next is a configuration that can support the use of fiat currency. The fiat market token economy works in similar fashion to the crypto market token economy. In this case, a fiat gateway (payment processing gateway) is used to buy and sell the market token exclusively using fiat currency. The market token is used within the market for NFT transactions and holds no value outside of the market. Note that fiat gateways bar the use of their services to buy or sell “digital currencies”. This configuration does not violate that term of service because the market token can’t be exchanged for any other crypto (such as BTC). The following diagram illustrates a fiat market token economy configuration.

![](<../../../../.gitbook/assets/APP-FS05-wallet-functions-Fiat Market Token.drawio.png>)

Figure 3. A fiat market token economy configuration.

### 6.4. Hybrid Market

This configuration combines the use of the crypto only economy and the fiat market token economy. These two configurations can work side by side while still operating within the service terms of fiat gateways. In this case, the market token can only be bought with or sold for fiat currencies. This prevents the ability to exchange the market token for another crypto. In addition, peers can still use the crypto they bring to the market to purchase NFTs. This is a complex economic configuration but also offers users the most flexibility for buying and selling NFTs. The following diagram illustrates a hybrid market economy configuration.

![](<../../../../.gitbook/assets/APP-FS05-wallet-functions-Hybrid Market.drawio.png>)

Figure 4. A hybrid market economy configuration.

### 6.5. Dual Market Token

As long as no exchange can occur between fiat and crypto currencies, the configuration is a viable option. It’s feasible to operate a market economy with two distinct market tokens: one for crypto, and another for fiat. The following diagram illustrates a dual market token economy configuration.

![](<../../../../.gitbook/assets/APP-FS05-wallet-functions-Dual Market Token.drawio.png>)

Figure 5. A dual market token economy configuration.

### 6.6. Why one market token can’t be exchanged with both crypto and fiat

Fiat gateways disallow the use of their services to buy or sell “digital currencies”. Basically, these services don’t want to be used as crypto exchanges and they don’t want to be used as entry points into the crypto economy at large. The reasons for this are outside the scope of this FS. Using any of the five configurations presented above will prevent any exchange between fiat and crypto currencies, thereby avoiding a violation of the terms of the fiat gateways. If the NFT Store were to accept both crypto and fiat for its market tokens, an exchange of fiat and crypto could occur and would violate the fiat gateway’s terms of service.

## 7. Process Overview

### 7.1. Crypto Processes

#### 7.1.1. Receiving crypto (deposit)

For an authenticated user in the NFT Store:

1. User opens the user session management menu in the app header or the wallet section of their user profile.
2. User selects a crypto asset under the wallet section.
3. User selects the “receive” option for their chosen crypto asset.
4. A “receive” menu opens which displays the user’s receipt address and instructions for receiving to that address.
5. User can copy the address to their clipboard with a button or similar control.
6. User then uses their receipt address off-site to send crypto to their address.
7. The user interface updates to show the crypto has been received.

#### 7.1.2. Sending crypto (withdraw)

For an authenticated user in the NFT Store that has a balance of some crypto asset:

1. User opens the user session management menu in the app header or the wallet section of their user profile.
2. User selects a crypto asset under the wallet section.
3. User selects the “send” option for their chosen crypto asset.
4. A “send” menu opens which displays a form with inputs.
5. User enters the amount of crypto they wish to send. The UI updates to show this value in a currency of the user’s choice.
6. User enters an account name to send the crypto. The UI may display some validation to the user about the account they enter here.
7. User may enter a memo if they wish. The user is guided by the UI about sending sidechain assets, if necessary.
8. User clicks the send button.
9. UI displays a confirmation page with the transaction details. The user clicks “confirm”.
10. UI displays a transaction success message to the user.
11. UI updates to show the new amount of assets owned by the user.

#### 7.1.3. Buying Crypto Market Tokens (CMTs)

Buying CMTs is how a peer would essentially deposit assets into their wallet to use for buying or bidding on NFTs, if not using other assets like BTC or PPY.

For an authenticated user in the NFT Store that has a balance of some crypto asset:

1. User initiates the CMT buying function within the user session management menu in the app header or the wallet section of their user profile.
2. UI displays a form to buy CMTs with crypto. User selects a valid crypto asset to use to buy CMTs. UI displays the exchange rate for this pair.
3. User enters how many CMTs they wish to buy, or how much crypto they’d like to spend. UI updates the other value.
4. User clicks the “buy” button.
5. UI displays a confirmation page with the transaction info. User clicks “confirm”.
6. UI displays a transaction success message to the user.
7. UI updates to show the new amount of assets owned by the user.

#### 7.1.4. Selling CMTs

Selling CMTs is how a tenant (or peer) would withdraw their crypto from the marketplace. One example is realizing revenue from NFT sales.

For an authenticated user in the NFT Store that has a balance of CMTs:

1. User initiates the CMT selling function within the wallet section of their user profile.
2. UI displays a form to sell CMTs for crypto. User selects a valid crypto asset to receive when selling CMTs. UI displays the exchange rate for this pair.
3. User enters how many CMTs they wish to sell, or how much crypto they’d like to receive. UI updates the other value.
4. User clicks the “sell” button.
5. UI displays a confirmation page with the transaction info. User clicks “confirm”.
6. UI displays a transaction success message to the user.
7. UI updates to show the new amount of assets owned by the user.

#### 7.1.5. Buying an NFT

This will be done using either crypto or a market token.

1. User initiates the NFT buying function within the NFT detail view or other page with the NFT buying function.
2. UI displays a form to buy the NFT with any valid asset (crypto, CMTs, FMTs). User selects a valid crypto asset to use to buy the NFT.
3. User clicks the “buy” button.
4. UI displays a confirmation page with the transaction info. User clicks “confirm”.
5. UI displays a transaction success message to the user.
6. UI updates to show the new amount of assets owned by the user.

### 7.2. Fiat Processes

Note that the NFT Store wallet doesn’t store fiat currency; it only stores crypto assets like coins, tokens, CMTs, FMTs, and NFTs. Movements of fiat currency are handled entirely by the fiat gateways.

#### 7.2.1. Buying Fiat Market Tokens (FMTs)

Buying FMTs is how a peer would essentially deposit assets into their wallet to use for buying or bidding on NFTs.

For an authenticated user in the NFT Store:

1. User initiates the FMT buying function within the user session management menu in the app header or the wallet section of their user profile.
2. UI displays a form to buy FMTs with crypto. User selects a valid fiat gateway to use to buy FMTs. UI displays the exchange rate for this pair.
3. User enters how many FMTs they wish to buy, or how much fiat they’d like to spend. UI updates the other value.
4. User clicks the “buy” button.
5. UI displays a confirmation page with the transaction info. User clicks “confirm”.
6. Depending on the fiat gateway, a purchase flow may occur on or off site. The user will then return to the NFT Store.
7. UI displays a transaction success message to the user.
8. UI updates to show the new amount of assets owned by the user.

#### 7.2.2. Selling FMTs

Selling FMTs is how a tenant (or peer) would withdraw their fiat from the marketplace. One example is realizing revenue from NFT sales.

For an authenticated user in the NFT Store that has a balance of FMTs:

1. User initiates the FMT selling function within the wallet section of their user profile.
2. UI displays a form to sell FMTs for fiat. User selects a valid fiat gateway to use to sell FMTs. UI displays the exchange rate for this pair.
3. User enters how many FMTs they wish to sell, or how much fiat they’d like to receive. UI updates the other value.
4. User clicks the “sell” button.
5. UI displays a confirmation page with the transaction info. User clicks “confirm”.
6. Depending on the fiat gateway, an invoice flow may occur on or off site. The user will then return to the NFT Store.
7. UI displays a transaction success message to the user.
8. UI updates to show the new amount of assets owned by the user.

## 8. Design Wire-frames

The wire-frames listed below are meant to represent the app’s wallet functions in various states. These are provided to assist in understanding of what features may look like or their potential use. Final designs may be vastly different from these images.

![](<../../../../.gitbook/assets/APP-FS05-wallet-functions-Wallet in App Header.drawio.png>)

Figure 6. Wallet functions in the App Header.

## 9. Requirements

Requirements specific to the items listed in this FS are as follows.

### 9.1. Wallet Functions

The wallet:

**APP-FS05-1:** shall exist on the Peerplays blockchain on the account associated with the user.

**APP-FS05-2:** shall store crypto assets which exist on the Peerplays blockchain, including but not limited to the following:

* crypto coins
* crypto tokens
* crypto market tokens
* fiat market tokens
* NFTs

**APP-FS05-3:** shall allow the transfer of owned assets (if the user is given the privileges to do so).

**APP-FS05-4:** shall allow the sale, auction, or exchange of owned assets (if the user is given the privileges to do so).

**APP-FS05-5:** shall allow the receipt of incoming assets.

### 9.2. Wallet in the App Header

The wallet, in the context of the app header:

**APP-FS05-6:** shall display market related assets (except for NFTs) owned by the user and provide the following details:

* asset name or icon
* asset symbol
* amount available
* value of the asset as per the user’s currency display settings

Note: Market related assets are any and all coins, tokens, CMTs, and FMTs that are configured for use within the NFT Store instance.

**APP-FS05-7:** shall provide the user with controls to quickly initiate sending and receiving of market related assets.

**APP-FS05-8:** shall display all fees to the user before the user confirms the transactions.

#### 9.2.1. CMTs

If the NFT Store is configured to use CMTs:

**APP-FS05-9:** shall display the amount of CMTs owned by the user (even if the amount is zero).

**APP-FS05-10:** shall provide the user with controls to quickly initiate buying (with crypto).

**APP-FS05-11:** while buying CMTs, shall provide the user the ability to specify the following options:

* which crypto asset they wish to use to buy CMTs or the Blockonomics gateway (if configured)
* either the amount of crypto asset they wish to spend or how much CMT they wish to buy

**APP-FS05-12:** while buying CMTs, shall provide the user with information about the transaction before the user confirms the transaction, including but not limited to the following:

* price per CMT
* amount of CMTs they are buying
* total price of the amount of CMTs they are buying
* transaction fees
* grand total of cost of the transaction
* expected amount of time the transaction will take to complete

#### 9.2.2. FMTs

If the NFT Store is configured to use FMTs:

**APP-FS05-13:** shall display the amount of FMTs owned by the user (even if the amount is zero).

**APP-FS05-14:** shall display the value of FMTs owned by the user (even if the amount is zero) in the display currency selected by the user.

**APP-FS05-15:** shall provide the user with controls to quickly initiate buying of FMTs (with fiat).

**APP-FS05-16:** while buying FMTs, shall provide the user the ability to specify the following options:

* which fiat gateway to use if multiple are configured
* either the amount of fiat they wish to spend or how much FMT they wish to buy

**APP-FS05-17:** while buying FMTs, shall provide the user with information about the transaction before the user confirms the transaction, including but not limited to the following:

* price per FMT
* amount of FMTs they are buying
* total price of the amount of FMTs they are buying
* transaction fees
* grand total of cost of the transaction
* expected amount of time the transaction will take to complete

### 9.3. Wallet in the User Profile

The wallet, in the context of user profile:

**APP-FS05-18:** shall display market related assets (except for NFTs) owned by the user and provide the following details:

* asset name
* asset icon
* asset symbol
* amount available
* value of the asset as per the user’s currency display settings

Note: Market related assets are any and all coins, tokens, CMTs, and FMTs that are configured for use within the NFT Store instance.

**APP-FS05-19:** shall provide the user with controls to quickly initiate sending and receiving of market related assets.

**APP-FS05-20:** shall display all fees to the user before the user confirms the transactions.

**APP-FS05-21:** shall display a history of asset sending and receiving activities, including but not limited to the following data:

* sender
* receiver
* asset
* amount
* memo
* Peerplays block number
* date
* time

**APP-FS05-22:** shall allow the user to view and / or download their private keys.

#### 9.3.1 Market Tokens

If the NFT Store is configured to use any market tokens (CMTs or FMTs):

**APP-FS05-23:** shall display a history of market token buying and selling activities, including but not limited to the following data:

* market token name
* purchase or sale
* amount of market token
* crypto asset sent or received (for CMTs)
* crypto asset value (for CMTs)
* fiat value (for FMTs)
* fiat transaction info and status (for FMTs)
* Peerplays block number
* date
* time

**APP-FS05-24:** market tokens shall have a precision of no greater than five (0.00001 theoretical minimum value) but may have less precision as per business needs.

**APP-FS05-25:** market tokens shall be created and exist on the Peerplays blockchain.

#### 9.3.2. CMTs

If the NFT Store is configured to use CMTs:

**APP-FS05-26:** shall display the amount of CMTs owned by the user (even if the amount is zero).

**APP-FS05-27:** shall provide the user with controls to quickly initiate buying and selling of CMTs (with and for crypto).

**APP-FS05-28:** while buying CMTs, shall provide the user the ability to specify the following options:

* which crypto asset they wish to use to buy CMTs or the Blockonomics gateway (if configured)
* either the amount of crypto asset they wish to spend or how much CMT they wish to buy

**APP-FS05-29:** while buying CMTs, shall provide the user with information about the transaction before the user confirms the transaction, including but not limited to the following:

* price per CMT
* amount of CMTs they are buying
* total price of the amount of CMTs they are buying
* transaction fees
* grand total of cost of the transaction
* expected amount of time the transaction will take to complete

**APP-FS05-30:** while selling CMTs, shall provide the user the ability to specify the following options:

* which crypto asset they wish to receive
* either the amount of crypto they wish to receive or how much CMT they wish to sell

**APP-FS05-31:** while selling CMTs, shall provide the user with information about the transaction before the user confirms the transaction, including but not limited to the following:

* price per CMT
* amount of CMTs they are selling
* total price of the amount of CMTs they are selling
* transaction fees
* grand total of revenue minus fees of the transaction
* expected amount of time the transaction will take to complete

#### 9.3.3. FMTs

If the NFT Store is configured to use FMTs:

**APP-FS05-32:** shall display the amount of FMTs owned by the user (even if the amount is zero).

**APP-FS05-33:** shall display the value of FMTs owned by the user (even if the amount is zero) in the display currency selected by the user.

**APP-FS05-34:** shall provide the user the ability to set up fiat withdrawal settings as required by each configured fiat gateway.

**APP-FS05-35:** shall provide the user with controls to quickly initiate buying and selling of FMTs (with and for fiat).

**APP-FS05-36:** while buying FMTs, shall provide the user the ability to specify the following options:

* which fiat gateway to use if multiple are configured
* either the amount of fiat they wish to spend or how much FMT they wish to buy

**APP-FS05-37:** while buying FMTs, shall provide the user with information about the transaction before the user confirms the transaction, including but not limited to the following:

* price per FMT
* amount of FMTs they are buying
* total price of the amount of FMTs they are buying
* transaction fees
* grand total of cost of the transaction
* expected amount of time the transaction will take to complete

**APP-FS05-38:** while selling FMTs, shall provide the user the ability to specify the following options:

* which fiat gateway to use if multiple are configured
* either the amount of fiat they wish to receive or how much FMT they wish to sell

**APP-FS05-39:** while selling FMTs, shall provide the user with information about the transaction before the user confirms the transaction, including but not limited to the following:

* price per FMT
* amount of FMTs they are selling
* total price of the amount of FMTs they are selling
* transaction fees
* grand total of revenue minus fees of the transaction
* expected amount of time the transaction will take to complete

### 9.4. Fiat Gateways

The wallet, in the context of fiat gateways:

**APP-FS05-40:** shall support the fait (payment) gateways that are built into the expressCart app, this currently includes:

* Paypal
* Stripe
* Authorize.net
* Adyen
* Westpac PayWay
* Zip

Note: Two payment gateway options are special cases:

* Instore (Used for tangible goods or transacting at a physical location)
* Blockonomics (Bitcoin and Bitcoin Cash)

These gateways should be included for the purpose of supporting sales of tangible goods (in the case of Instore) or allowing the purchase of CMTs with Blockonomics. Also note that FMTs should not be bought or sold through Blockonomics as that would allow an exchange of fiat and crypto and break the terms of other fiat gateways.

## 10. Appendix A: Glossary

| Term   | Meaning                    |
| ------ | -------------------------- |
| RS     | Requirements Specification |
| FS     | Functional Specification   |
| NFT(s) | Non-Fungible Token(s)      |
| FMT(s) | Fiat Market Token(s)       |
| CMT(s) | Crypto Market Token(s)     |
| UI     | User Interface             |
| UX     | User Experience            |
