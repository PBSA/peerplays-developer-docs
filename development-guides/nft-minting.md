---
description: A guide for creating NFTs from start to finish.
---

# NFT Minting

## 1. Creating Non-Fungible Tokens (NFTs)

Creating NFTs in Peerplays is a two step process.

First, the metadata must be created for the NFT. This can be thought of like a template from which NFTs can be constructed. The account which owns the NFT metadata is the account with the rights to mint (create) the actual NFTs based on that metadata. The metadata is used to set certain basic permissions for the NFT, revenue sharing options, and the NFT identity information (name, symbol, and a URI).

Then using the metadata, NFTs can be minted into existence by the metadata owner. Once the NFTs are minted, they can be transferred, sold, auctioned, or otherwise used for their intended purpose as the permissions in the metadata allow.

## 2. NFT Metadata

### 2.1. nft\_metadata\_create

This function creates a new NFT metadata object. It can be created with or without a set of permissions. See the guide on permissions for details about [creating resource permissions](introduction-to-permissions.md#3-resource-permissions-with-account-roles).

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::nft_metadata_create(
  string owner_account_id_or_name,
  string name,
  string symbol,
  string base_uri,
  optional<string> revenue_partner,
  optional<uint16_t> revenue_split,
  bool is_transferable,
  bool is_sellable,
  optional<account_role_id_type> role_id,
  bool broadcast)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `nft_metadata_create` function looks like this:

{% code title="When using the cli_wallet..." %}
```
nft_metadata_create <owner_account_id_or_name> <name> <symbol> <base_uri> <revenue_partner> <revenue_split> <is_transferable> <is_sellable> <role_id> true
```
{% endcode %}

#### Parameters <a href="parameters" id="parameters"></a>

| name                         | data type                          | description                                                                                                                                                                           |
| ---------------------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| owner\_account\_id\_or\_name | string                             | The name or id of the account who is creating the metadata.                                                                                                                           |
| name                         | string                             | The name of the NFTs created from this metadata.                                                                                                                                      |
| symbol                       | string                             | The symbol of the NFTs created from this metadata.                                                                                                                                    |
| base\_uri                    | string                             | The URI of the NFT. This could be a website link, an API call, something on IPFS, some other unique identifier, etc.                                                                  |
| revenue\_partner             | string (optional)                  | The account name of the revenue partner. Whenever the minted NFT is sold in the marketplace a split of the selling price goes to the revenue partner. This can be the metadata owner. |
| revenue\_split               | uint16\_t (optional)               | The amount of the selling price that goes to the revenue partner. 10000 = 100%, 100 = 1%, 1 = 0.01%                                                                                   |
| is\_transferable             | bool                               | Specifies if the minted NFTs can be transferred by the owner of the NFT to other accounts.                                                                                            |
| is\_sellable                 | bool                               | Specifies if the minted NFTs can be sold in the marketplace.                                                                                                                          |
| role\_id                     | account\_role\_id\_type (optional) | The resource permissions specifying the whitelisted accounts among which the NFTs can either be transferred or sold on the marketplace.                                               |
| broadcast                    | bool                               | `true` or `false`, whether or not you want to broadcast the transaction to the network.                                                                                               |

#### Example Call Without Permissions <a href="example-call-without-permissions" id="example-call-without-permissions"></a>

```cpp
nft_metadata_create account01 "AVALON NAME" "AVALON SYMBOL" "avalonmeta.com" account02 150 false true null true
```

In the example above:

* **account01** is the owner account of the metadata.
* **"AVALON NAME"** is the name of the NFTs created from this NFT metadata.
* **"AVALON SYMBOL"** is the symbol of the NFTs created from this NFT metadata. The symbol is reserved for future use.
* **"avalonmeta.com"** is the URI of the NFTs.
* **account02** is the revenue partner.
* **150** is the revenue\_split that will be shared with the revenue\_partner. In this case, 1.5%.
* **false**, is\_transferable specifies the minted NFTs cannot be transferred by the owner of the NFT to other accounts.
* **true**, is\_sellable specifies minted NFTs can be sold in the marketplace.
* **null**, role\_id is not set.
* **true**, This transaction will be in the upcoming block on the chain.

#### Example Call With Permissions <a href="example-call-with-permissions" id="example-call-with-permissions"></a>

First we create the permission:

```cpp
create_account_role account01 "Avalon Permissions1" "Permission Metadata" [88,89,90,95] [1.2.40, 1.2.41] "2020-11-04T13:43:39" true
```

In the above function...

* **account01** is the owner of the permission being created which can be attached to NFT metadata.
* **"Avalon Permissions1"** is the name of the resource permission being created.
* **"Permission Metadata"** is the metadata, can be a JSON object as well.
* **\[88,89,90,95]** is the list of operations this permission whitelists for accounts.
  * 88 - List Offer operation in marketplace
  * 89 - Bid operation in marketplace
  * 90 - Cancel Offer operation in marketplace
  * 95 - Safe transfer from owner to another NFT
  * IDs are [available here](../supporting-and-reference-docs/operation-ids-list.md).
* **\[1.2.40, 1.2.41]** List of accounts whitelisted for the above operations. Offer, bid and transfer operations can only be among the whitelisted accounts.
* **"2020-11-04T13:43:39"** Expiry date of the permission
* **true** broadcast, keep it true to include the transaction in upcoming block.

Then we create the metadata with the above permission:

```cpp
nft_metadata_create account01 "AVALON NAME" "AVALON SYMBOL" "avalonmeta.com" account02 150 false true 1.32.0 true
```

This example is just like the previous, except this time we include **1.32.0**, the role\_id, to attach permissions to all the NFTs created from this metadata.
{% endtab %}
{% endtabs %}

### 2.2. nft\_metadata\_update

This function updates an existing NFT metadata object.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::nft_metadata_update(
  string owner_account_id_or_name,
  nft_metadata_id_type nft_metadata_id,
  optional<string> name,
  optional<string> symbol,
  optional<string> base_uri,
  optional<string> revenue_partner,
  optional<uint16_t> revenue_split,
  optional<bool> is_transferable,
  optional<bool> is_sellable,
  optional<account_role_id_type> role_id,
  bool broadcast)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `nft_metadata_update` function looks like this:

{% code title="When using the cli_wallet..." %}
```
nft_metadata_update <owner_account_id_or_name> <nft_metadata_id> <name> <symbol> <base_uri> <revenue_partner> <revenue_split> <is_transferable> <is_sellable> <role_id> true
```
{% endcode %}

#### Parameters <a href="parameters" id="parameters"></a>

| name                         | data type               | description                                                                             |
| ---------------------------- | ----------------------- | --------------------------------------------------------------------------------------- |
| owner\_account\_id\_or\_name | string                  | The name or id of the account who owns the metadata this NFT is based on.               |
| nft\_metadata\_id            | nft\_metadata\_id\_type | The ID of the metadata you want to update.                                              |
| name                         | string (optional)       | A new name for the NFT.                                                                 |
| symbol                       | string (optional)       | A new symbol for the NFT.                                                               |
| base\_uri                    | string (optional)       | A new URI of the NFT.                                                                   |
| revenue\_partner             | string (optional)       | A new revenue partner account name or id.                                               |
| revenue\_split               | uint16\_t (optional)    | A new revenue split amount.                                                             |
| is\_transferable             | bool (optional)         | Whether or not the minted NFTs are transferable.                                        |
| is\_sellable                 | bool (optional)         | Whether or not the minted NFTs are sellable in the marketplace.                         |
| role\_id                     | account\_role\_id\_type | A new set of resource permissions.                                                      |
| broadcast                    | bool                    | `true` or `false`, whether or not you want to broadcast the transaction to the network. |

#### Example Call <a href="example-call" id="example-call"></a>

```cpp
nft_metadata_update account01 1.30.1 sknft01 sknft01 sknft01 null null true true true
```
{% endtab %}
{% endtabs %}

## 3. Minting NFTs

### 3.1. nft\_create

This function mints an NFT based on the metadata. NFTs are always minted by the owner of the metadata object the NFTs are based on. The fees associated with minting an NFT is just the transaction fee + storage fee which are both collected in core PPY token. After an NFT has been minted it can be sold on the marketplace for any token of the NFT owner’s choice.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::nft_create(
  string metadata_owner_account_id_or_name,
  nft_metadata_id_type metadata_id,
  string owner_account_id_or_name,
  string approved_account_id_or_name,
  string token_uri,
  bool broadcast)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `nft_create` function looks like this:

{% code title="When using the cli_wallet..." %}
```
nft_create <metadata_owner_account_id_or_name> <metadata_id> <owner_account_id_or_name> <approved_account_id_or_name> <token_uri> true
```
{% endcode %}

#### Parameters <a href="parameters" id="parameters"></a>

| name                                   | data type               | description                                                                                                                                                                                                                                                              |
| -------------------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| metadata\_owner\_account\_id\_or\_name | string                  | The name or id of the account who owns the metadata this NFT is based on.                                                                                                                                                                                                |
| metadata\_id                           | nft\_metadata\_id\_type | The NFT metadata object ID to base this NFT on. (Currently this works on the "best guess" method by doing `get_object` 1.30.0, 1.30.1, 1.30.2... and so on. In the future a new cli command will be introduced that can list all the metadata objects by owner account.) |
| owner\_account\_id\_or\_name           | string                  | The owner of the freshly minted NFT. As the owner of the metadata, you can mint NFTs directly into someone elses account!                                                                                                                                                |
| approved\_account\_id\_or\_name        | string                  | The approved account of the NFT, can be the owner as well.                                                                                                                                                                                                               |
| token\_uri                             | string                  | The URI of the NFT. This could be a website link, an API call, something on IPFS, some other unique identifier, etc.                                                                                                                                                     |
| broadcast                              | bool                    | `true` or `false`, whether or not you want to broadcast the transaction to the network.                                                                                                                                                                                  |

#### Example Call <a href="example-call" id="example-call"></a>

```cpp
nft_create account01 1.30.0 account02 account02 "AVALON NFT URI" true
```

In the example above:

* **account01** is the owner account of the metadata.
* **1.30.0** is the ID of the metadata object.
* **account02** is the owner of the minted NFT.
* **account02** is the approved account of the minted NFT.
* **AVALON NFT URL** is the URI that has additional information about this NFT.
* **true**, This transaction will be in the upcoming block on the chain.
{% endtab %}
{% endtabs %}

## 4. Sending NFTs

### 4.1. nft\_transfer\_from (nft\_safe\_transfer\_from)

These functions are used to transfer NFTs from one account to another, if allowed. The difference between the two is that the safe transfer includes a string parameter which can be used to send along some metadata (or a memo) with the NFT. Otherwise, they are the same.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::nft_transfer_from(
  string operator_account_id_or_name,
  string from_account_id_or_name,
  string to_account_id_or_name,
  nft_id_type token_id,
  bool broadcast)
```
{% endcode %}

**Or...**

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::nft_safe_transfer_from(
  string operator_account_id_or_name,
  string from_account_id_or_name,
  string to_account_id_or_name,
  nft_id_type token_id,
  string data,
  bool broadcast)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `nft_transfer_from` and `nft_safe_transfer_from` function looks like this:

{% code title="When using the cli_wallet..." %}
```
nft_transfer_from <operator_account_id_or_name> <from_account_id_or_name> <to_account_id_or_name> <token_id> true

nft_safe_transfer_from <operator_account_id_or_name> <from_account_id_or_name> <to_account_id_or_name> <token_id> <data> true
```
{% endcode %}

#### Parameters <a href="parameters" id="parameters"></a>

| name                            | data type     | description                                                                                       |
| ------------------------------- | ------------- | ------------------------------------------------------------------------------------------------- |
| operator\_account\_id\_or\_name | string        | The operator or owner account.                                                                    |
| from\_account\_id\_or\_name     | string        | The account sending the NFT.                                                                      |
| to\_account\_id\_or\_name       | string        | The account receiving the NFT.                                                                    |
| token\_id                       | nft\_id\_type | The ID of the NFT.                                                                                |
| data                            | string        | Only for `nft_safe_transfer_from`, you can include more metadata in the transfer with this field. |
| broadcast                       | bool          | `true` or `false`, whether or not you want to broadcast the transaction to the network.           |

#### Example Call <a href="example-call" id="example-call"></a>

```cpp
nft_safe_transfer_from account02 account02 account03 1.31.0 "Enjoy my NFT" true
```

In the example above:

* **account02** is the owner or operator account.
* **account02** is sending the NFT.
* **account03** is receiving the NFT.
* **1.31.0** is the ID of the NFT.
* **"Enjoy my NFT"** is the metadata of the transfer.
* **true**, This transaction will be in the upcoming block on the chain.
{% endtab %}
{% endtabs %}

## 5. Related Documents

The functions listed in this guide will cost transaction fees. To calculate how much PPY you'll need to make these transactions and meet your development goals, please see the [Calculating Costs](calculating-costs.md) guide.
