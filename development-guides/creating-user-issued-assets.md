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

A **User Issued Asset** allows the issuer to control the supply of the asset. A Market Issued Asset \(MIA, and also known as a Market Pegged Asset, MPA, or Smart Coin\) puts the control of the asset's supply in the hands of the market.

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
create_asset <issuer> <symbol> <precision> <common> null true
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

The `asset_options` object contains the options that are common to all assets. This is why it's necessary to supply for both UIAs and MIAs. The options need to be passed as a raw JSON object that contains these settings:

```text
{
   "max_supply" : <number>
   "market_fee_percent" : <number>
   "max_market_fee" : <number>
   "issuer_permissions" : <permissions>,
   "flags" : <flags>,
   "core_exchange_rate" : {
       "base": {
         "amount": <number>,
         "asset_id": "1.3.0"
       },
       "quote": {
         "amount": <number>,
         "asset_id": <this asset's ID>
       }
   },
   "whitelist_authorities" : [],
   "blacklist_authorities" : [],
   "whitelist_markets" : [],
   "blacklist_markets" : [],
   "description" : <string>
}
```

Let's break these down one-by-one. Pay special attention to the permissions and flags as they are the most complicated part of the options object.

<table>
  <thead>
    <tr>
      <th style="text-align:left">key</th>
      <th style="text-align:left">description</th>
      <th style="text-align:left">example value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">max_supply</td>
      <td style="text-align:left">
        <p>The maximum amount of this asset that can exist in circulation. This number
          is in Satoshi.
          <br />So if you want a max supply of 100 tokens, and the asset has a precision
          of 2, the max_supply value should be 10000.</p>
        <p>100.00 (100 tokens with precision of 2)</p>
        <p>10000 (and remove the decimal point)</p>
      </td>
      <td style="text-align:left">10000</td>
    </tr>
    <tr>
      <td style="text-align:left">market_fee_percent</td>
      <td style="text-align:left">
        <p>If market fees of a UIA are turned on, fees have to be payed for each
          market transaction. The issuer can claim these fees. If the fee is set
          to 1%, the issuer will earn 1% of market volume as profit.</p>
        <p>
          <br />This is set in hundredths of one percent.
          <br />So if you want 0.3%...</p>
        <p>0.3% = 0.003</p>
        <p>(hundredth of one percent) 100 / 0.01 = 10,000</p>
        <p>0.003 * 10,000 = 30 (our value for the 0.3% fee we want)</p>
      </td>
      <td style="text-align:left">30</td>
    </tr>
    <tr>
      <td style="text-align:left">max_market_fee</td>
      <td style="text-align:left">
        <p>The maximum amount to charge for any market transaction. This is set in
          Satoshi as well.</p>
        <p>If you want the max fee to be only 1 token...</p>
        <p>1.00 (1 token with precision of 2)</p>
        <p>100 (and remove the decimal point)</p>
      </td>
      <td style="text-align:left">100</td>
    </tr>
    <tr>
      <td style="text-align:left">issuer_permissions</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">flags</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">core_exchange_rate</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">whitelist_authorities</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">blacklist_authorities</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">whitelist_markets</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">blacklist_markets</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">description</td>
      <td style="text-align:left">A string that describes the asset.</td>
      <td style="text-align:left">&quot;My fancy new token&quot;</td>
    </tr>
  </tbody>
</table>

### 1.5. Issuing Assets

So far creating assets with `create_asset` doesn't actually produce the new tokens into anyone's account. For that we use the `issue_asset` function.

#### issue\_asset

Issues new shares of an asset that exists via `create_asset`.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::issue_asset(
    string to_account, 
    string amount, 
    string symbol, 
    string memo, 
    bool broadcast = false)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `issue_asset` function looks like this:

{% code title="When using the cli\_wallet..." %}
```text
issue_asset <to_account> <amount> <symbol> <memo> true
```
{% endcode %}

#### Parameters

| name | data type | description | details |
| :--- | :--- | :--- | :--- |
| to\_account | string | The name or id of the account to receive the new shares. | n/a |
| amount | string | The amount to issue, in nominal units. | Example: 0.5 for half a token |
| symbol | string | The ticker symbol of the asset to issue. | n/a |
| memo | string | A memo to include in the transaction, readable by the recipient. | n/a |
| broadcast | bool | `true` or `false`, whether or not you want to broadcast the transaction. | n/a |

#### Example Call

```text
issue_asset "myfriend1" 1000 "BTFUN" "Enjoy some BitFun, Friend!" true
```
{% endtab %}
{% endtabs %}

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

