---
title: Creating User Issued Assets
description: >-
  A guide to create UIAs, also known as Fan Tokens or Community Asset Tokens
  (CATs).
---

# Creating User Issued Assets

## 1. Creating User Issued Assets \(UIAs\)

There are many reasons to create assets. Peerplays allows individuals and companies to create and issue their own tokens for anything they can imagine. The potential use cases for these User Issued Assets \(UIAs\) are innumerable. On the one hand, UIAs can be used as simple event tickets deposited on the customers mobile phone to enter a concert or movie. On the other hand, they can be used for crowd funding, ownership tracking, or even to sell equity of a company in form of stock. UIAs are incredibly versatile and can help solve a plethora of use cases like...

* event tickets
* reputation points
* loyalty reward points
* flight miles
* company shares
* fan credits
* crowd funding
* digital property

All you need to do is to define your preferred parameters for your coin, such as supply, precision, symbol, and description to see your coin’s birth after only a few seconds. From that point on, you can issue some of your coins to whomever you want, sell them, and see them instantly **traded against any other existing coin** on Peerplays.

Unless you want some restrictions, as the issuer, you have certain privileges over your coin. For instance, you can allow trading only in certain market pairs and define who actually is allowed to hold your coin by using whitelists and blacklists. An issuer can opt-out of their privileges indefinitely for the sake of trust and reputation.

As the owner of your new coin, you don’t need to take care of all the technical details of blockchain technology, such as distributed consensus algorithms, blockchain development, or integration. You don’t even need to run any mining equipment or servers, at all.

The regulations that apply to each kind of token vary widely and are often different in every jurisdiction. Hence, Peerplays comes with tools that allow issuers to remain compliant with all applicable regulations when issuing assets assuming regulators allow such assets in the first place.

### 1.1. User Issued Assets vs. Market Issued Assets

A **User Issued Asset** allows the issuer to control the supply of the asset. A Market Issued Asset \(also known as a Market Pegged Asset, MPA, or Smart Coin\) puts the control of the asset's supply in the hands of the market.

**Market issued assets** are used for pegging the value of an asset to another underlying asset. This type of asset requires collateral to back its value and a fair market price feed so the market can automatically trigger margin calls to balance the market asset value. Market issued assets will be the subject of another guide.

User issued assets are controlled and distributed by the issuing user. This guide will focus on the creation of UIAs.

### 1.2. Creating Assets

#### create\_asset

This function creates a new user issued or market issued asset. Many of the asset's options can be changed later using `update_asset`. You must provide raw JSON data structures for the `options` object.

{% tabs %}
{% tab title="Structure" %}
The basic structure of the `create_asset` function looks like this:

{% code title="When using the cli\_wallet..." %}
```text
create_asset <issuer> <symbol> <precision> <options> null false
```
{% endcode %}

More specifically:

```cpp
signed_transaction graphene::wallet::wallet_api::create_asset(
    string issuer, 
    string symbol, 
    uint8_t precision, 
    asset_options common, 
    fc::optional<bitasset_options> bitasset_opts, 
    bool broadcast = false)
```
{% endtab %}

{% tab title="Parameters" %}
* **`issuer`**: the name or id of the account who will pay the fee and become the issuer of the new asset. This can be updated later.
* **`symbol`**: the ticker symbol of the new asset
* **`precision`**: the number of digits of precision to the right of the decimal point, must be less than or equal to 12
* **`common`**: asset options required for all new assets. Note that core\_exchange\_rate technically needs to store the asset ID of this new asset. Since this ID is not known at the time this operation is created, create this price as though the new asset has instance ID 1, and the chain will overwrite it with the new asset’s ID.
* **`bitasset_opts`**: options specific to BitAssets. This may be null unless the `market_issued` flag is set in common.flags
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}
{% endtabs %}

#### Parameters



{% hint style="danger" %}
**Important Note:**

The **`null`** for the second to last parameter is essential for making a **user issued asset**. 

A set of curly braces, **`{}`** for the parameter could be used to construct a **market issued asset**, but that is the subject of another guide.
{% endhint %}

### 1.3. Updating Assets

### 1.4. Asset Options

### 1.5. Issuing Assets

## 2. Reserved Tokens

Some token names \(symbols\) are reserved by the blockchain because they are assets that already exist, on or off-chain. This is to avoid confusion and prevent scams. The most popular assets have been reserved and are listed below. The symbols listed here are controlled by the Peerplays SONs `son-account`. If a genuine user wants to create an asset with the symbol, the SONs will raise a proposal and the Witnesses will vote to transfer the ownership to the user.

### 2.1. List of Reserved Tokens

```text
AAVE
ADA
ALGO
AMP
ATOM
AVAX
BCH
BNB
BSV
BTCB
BTS
BUSD
CAKE
CRO
DAI
DOGE
DOT
EGR
EOS
ETC
ETH
FEI
FIL
FTT
HBD
HIVE
ICP
KLAY
LEO
LINK
LTC
LUNA
MATIC
MIOTA
MKR
NEO
SBD
SHIB
SOL
STEEM
TFUEL
THETA
TRX
UNI
USDC
USDT
UST
VET
WBNB
WBTC
XLM
XMR
XRP
XTZ
```

