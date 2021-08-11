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

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::create_asset(
    string issuer, 
    string symbol, 
    uint8_t precision, 
    asset_options common, 
    fc::optional<bitasset_options> bitasset_opts, 
    bool broadcast = false)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `create_asset` function looks like this:

{% code title="When using the cli\_wallet..." %}
```text
create_asset <issuer> <symbol> <precision> <options> null true
```
{% endcode %}

#### Parameters

| name | data type | description | details |
| :--- | :--- | :--- | :--- |
| issuer | string | The name or id of the account who will pay the fee and become the issuer of the new asset. This can be updated later. | n/a |
| symbol | string | The ticker symbol of the new asset. | n/a |
| precision | uint8\_t | The number of digits of precision to the right of the decimal point, must be between 0 and 12 \(inclusive\). | min 0, max 12 |
| common | asset\_options | Asset options required for all new assets. See section [1.4. Asset Options](creating-user-issued-assets.md#1-4-asset-options) for details. | n/a |
| bitasset\_opts | bitasset\_options | Options specific to market issued assets. Put `null` here for a UIA. Put `{}` here for a market issued asset with the default price feed settings. Or you can price feed details here. | `null` for UIAs! |
| broadcast | bool | `true` or `false`, whether or not you want to broadcast the transaction. | n/a |

#### Example Call

```text
create_asset "1.2.18" "BTFUN" 8 {"max_supply" : 1000000000000000,"market_fee_percent" : 0,"max_market_fee" : 1000000000000000,"issuer_permissions" : 79,"flags" : 0,"core_exchange_rate" : {"base": {"amount": 1,"asset_id": "1.3.0"},"quote": {"amount": 1,"asset_id": "1.3.1"}},"whitelist_authorities" : [],"blacklist_authorities" : [],"whitelist_markets" : [],"blacklist_markets" : [],"description" : "BitFun token with precision 8"} null true
```

In this example call we used the following settings for the `common` parameter:

```text
{
  "max_supply" : 1000000000000000,
  "market_fee_percent" : 0,
  "max_market_fee" : 1000000000000000,
  "issuer_permissions" : 79,
  "flags" : 0,
  "core_exchange_rate" : {
    "base": {
      "amount": 1,
      "asset_id": "1.3.0"
    },
    "quote": {
      "amount": 1,
      "asset_id": "1.3.1"
    }
  },
  "whitelist_authorities" : [],
  "blacklist_authorities" : [],
  "whitelist_markets" : [],
  "blacklist_markets" : [],
  "description" : "BitFun token with precision 8"
}
```

Please see section [1.4. Asset Options](creating-user-issued-assets.md#1-4-asset-options) for an explanation of the `common` parameter!
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
**Important Note:**

The **`null`** for the second to last parameter is essential for making a **user issued asset**. 

A set of curly braces, **`{}`** for the parameter could be used to construct a **market issued asset**, but that is the subject of another guide.
{% endhint %}

### 1.3. Updating Assets

A UIA can be modified by the issuer after its creation. A separate call, `update_asset`, has been created for this purpose.

What can and cannot be changed? Except for the **symbol** and **precision**, every parameter, option, or setting can be updated.

{% hint style="danger" %}
Note that once a permission is removed, it can **never** be re-enabled again!
{% endhint %}

#### update\_asset

This function updates the core options on an asset. There are a number of options which all assets in the network use. These options are enumerated in the `asset_object::asset_options` struct. This command is used to update these options for an existing asset.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::update_asset(
    string symbol, 
    optional<string> new_issuer, 
    asset_options new_options, 
    bool broadcast = false)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `update_asset` function looks like this:

{% code title="When using the cli\_wallet..." %}
```text
update_asset <symbol> <new_issuer> <new_options> true
```
{% endcode %}

#### Parameters

| name | data type | description | details |
| :--- | :--- | :--- | :--- |
| symbol | string | The ticker symbol of the existing asset being updated. | n/a |
| new\_issuer | string | If changing the asset’s issuer, the name or id of the new issuer. `null` if you wish to remain the issuer of the asset. | n/a |
| new\_options | asset\_options | The new asset\_options object, which will entirely replace the existing options. See section [1.4. Asset Options](creating-user-issued-assets.md#1-4-asset-options) for details. | n/a |
| broadcast | bool | `true` or `false`, whether or not you want to broadcast the transaction. | n/a |

#### Example Call

```text
update_asset "BTFUN" null {"max_supply" : 1000000000000000,"market_fee_percent" : 50,"max_market_fee" : 1000000000000000,"issuer_permissions" : 79,"flags" : 0,"core_exchange_rate" : {"base": {"amount": 1,"asset_id": "1.3.0"},"quote": {"amount": 1,"asset_id": "1.3.1"}},"whitelist_authorities" : [],"blacklist_authorities" : [],"whitelist_markets" : [],"blacklist_markets" : [],"description" : "BitFun token for fun!"} true
```

In this example call we used the following settings for the `common` parameter:

```text
{
  "max_supply" : 1000000000000000,
  "market_fee_percent" : 50,
  "max_market_fee" : 1000000000000000,
  "issuer_permissions" : 79,
  "flags" : 0,
  "core_exchange_rate" : {
    "base": {
      "amount": 1,
      "asset_id": "1.3.0"
    },
    "quote": {
      "amount": 1,
      "asset_id": "1.3.1"
    }
  },
  "whitelist_authorities" : [],
  "blacklist_authorities" : [],
  "whitelist_markets" : [],
  "blacklist_markets" : [],
  "description" : "BitFun token for fun!"
}
```

Please see section [1.4. Asset Options](creating-user-issued-assets.md#1-4-asset-options) for an explanation of the `common` parameter!
{% endtab %}
{% endtabs %}

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

