---
title: Intro to Permissions
description: >-
  A brief explanation of role-based & resource permissions for Peerplays
  objects.
---

# Introduction to Permissions

## 1. Types of Permissions in Peerplays

There are three functions available in the CLI Wallet to produce permission settings:

1. **`create_custom_permission`**
2. **`create_custom_account_authority`**
3. **`create_account_role`**

The first two functions deal with creating Hierarchical Role-based Permissions \(HRP\). The third is used to provide resource permissions with account roles.

HRP is a feature of the Peerplays blockchain which helps to increase the security of user accounts. Users don’t have to use their `active` and `owner` keys for everything they do on the chain. Instead, they can create role based custom permissions and map them to different keys other than `active` and `owner` keys. They can then use these custom keys to sign transactions.

As opposed to HRP mentioned above, resource permissions are controlled by the owner of a resource \(such as NFT metadata\). These are similar to IAM permissions in an AWS Cloud environment.

## 2. Hierarchical Role-based Permissions \(HRP\)

To create a custom HRP, the first step is to create the custom permission. Then the custom permission will be mapped to the actual operations present on the blockchain by creating a related custom account authority.

### 2.1. create\_custom\_permission

This function creates a new custom permission.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::create_custom_permission(
    string owner, 
    string permission_name, 
    authority auth, 
    bool broadcast)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `create_custom_permission` function looks like this:

{% code title="When using the cli\_wallet..." %}
```text
create_custom_permission <owner> <permission_name> <auth> true
```
{% endcode %}

#### Parameters <a id="parameters"></a>

| name | data type | description | details |
| :--- | :--- | :--- | :--- |
| owner | string | The name or id of the account who is creating the permission. | n/a |
| permission\_name | string | The name of the permission. | n/a |
| auth | authority | This is a JSON object which describes an account authority. See below for details. | a JSON object |
| broadcast | bool | `true` or `false`, whether or not you want to broadcast the transaction. | n/a |

The `auth` parameter is a JSON object which looks like this:

```cpp
{
  "weight_threshold": 1,
  "account_auths": [["1.2.52",1]],
  "key_auths": [["TEST71ADtL4fzjGKErk9nQJrABmCPUR8QCjkCUNfdmgY5yDzQGhwto",1]],
  "address_auths": []
}
```

This represents an authority structure, `account_auths` represents the amount of weight each account has on our account, in this example `1.2.52` has a weight of 1.

`key_auths` represents the amount of weight each public key has on this account, in this example `TEST71ADtL4fzjGKErk9nQJrABmCPUR8QCjkCUNfdmgY5yDzQGhwto` has a weight of 1.

`weight_threshold` represents the required weight for a transaction to be signed successfully.

In this example, either `1.2.52` can sign with their active key or `TEST71ADtL4fzjGKErk9nQJrABmCPUR8QCjkCUNfdmgY5yDzQGhwto` can be used to sign a transaction successfully because they each have a weight of 1, and only 1 is required by the threshold.

#### Example Call <a id="example-call"></a>

```cpp
create_custom_permission account01 perm1 { "weight_threshold": 1,  "account_auths": [["1.2.52",1]], "key_auths": [["TEST71ADtL4fzjGKErk9nQJrABmCPUR8QCjkCUNfdmgY5yDzQGhwto",1]], "address_auths": [] } true
```
{% endtab %}
{% endtabs %}

### 2.2. get\_custom\_permissions

This function returns the custom permissions that have been created by the given account.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::get_custom_permissions(
    string owner)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `get_custom_permission` function looks like this:

{% code title="When using the cli\_wallet..." %}
```text
get_custom_permissions <owner>
```
{% endcode %}

#### Parameters <a id="parameters"></a>

| name | data type | description | details |
| :--- | :--- | :--- | :--- |
| owner | string | The name or id of the account for which we'd like to see the list of created custom permissions. | n/a |

#### Example Call

```cpp
get_custom_permissions account01
```
{% endtab %}
{% endtabs %}

### 2.3. update\_custom\_permission

This function updates an existing permission with the new authority object that you supply.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::update_custom_permission(
    string owner,
    custom_permission_id_type permission_id,
    fc::optional<authority> new_auth,
    bool broadcast)
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
The basic structure of the `update_custom_permission` function looks like this:

{% code title="When using the cli\_wallet..." %}
```text
update_custom_permissions <owner> <permission_id> <new_auth> true
```
{% endcode %}

#### Parameters <a id="parameters"></a>

| name | data type | description | details |
| :--- | :--- | :--- | :--- |
| owner | string | The name or id of the account who is updating the permission. | n/a |
| permission\_id | custom\_permission\_id\_type | The ID of the custom permission we're intending to edit. | n/a |
| new\_auth | authority | Just like [create\_custom\_permission](introduction-to-permissions.md#2-1-create_custom_permission), this is a JSON object which represents an account authority. | n/a |
| broadcast | bool | `true` or `false`, whether or not you want to broadcast the transaction. | n/a |

#### Example Call

```cpp
update_custom_permission account01 1.27.0 { "weight_threshold": 1,  "account_auths": [["1.2.53",1]], "key_auths": [], "address_auths": [] } true
```

Here we removed the `key_auths` and added `1.2.53` with weight 1, which is equal to `weight_threshold` , so `1.2.53` can alone sign the transaction successfully.
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
The new authority object will **replace** the old authority object using this function call. Make sure that the authority object that you supply here is set exactly as you'd like.
{% endhint %}

### 2.4. create\_custom\_account\_authority

Creating a custom authority maps the created [custom permissions](introduction-to-permissions.md#2-1-create_custom_permission) with the actual operations present on the blockchain. It also has expiry time by when this custom permission is no longer valid on any given account and operation combination.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::create_custom_account_authority(
    string owner,
    custom_permission_id_type permission_id,
    int operation_type,
    fc::time_point_sec valid_from,
    fc::time_point_sec valid_to,
    bool broadcast)
```
{% endcode %}



### 2.5. update\_custom\_account\_authority

This function can be used to update existing `valid_from` and `valid_to` times.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::update_custom_account_authority(
    string owner,
    custom_account_authority_id_type auth_id,
    fc::optional<fc::time_point_sec> new_valid_from,
    fc::optional<fc::time_point_sec> new_valid_to,
    bool broadcast)
```
{% endcode %}



### 2.6. delete\_custom\_permission

This function is used to delete an existing custom permission.

{% hint style="warning" %}
This will delete all the custom account authorities linked to this permission as well.

A cascading delete!
{% endhint %}

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::delete_custom_permission(
    string owner,
    custom_permission_id_type permission_id,
    bool broadcast)
```
{% endcode %}



### 2.7. delete\_custom\_account\_authority

This function is used to delete an account authority attached to a permission.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::delete_custom_account_authority(
    string owner,
    custom_account_authority_id_type auth_id,
    bool broadcast)
```
{% endcode %}



## 3. Resource Permissions with Account Roles

Resource permissions can be granted by applying account roles to those resources. The allowed permissions are attached to the role. Then accounts can be added to the role to grant them the permissions the role provides.

### 3.1. create\_account\_role

This function creates an account role.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::create_account_role(
    string owner_account_id_or_name,
    string name,
    string metadata,
    flat_set<int> allowed_operations,
    flat_set<account_id_type> whitelisted_accounts,
    time_point_sec valid_to,
    bool broadcast)
```
{% endcode %}



### 3.2. get\_account\_roles\_by\_owner

You can use this to find all the account roles by their owner.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::get_account_roles_by_owner(
    string owner_account_id_or_name)
```
{% endcode %}



### 3.3. update\_account\_role

As a resource owner, you can update the operations and whitelisted accounts present in an account role. This helps in blacklisting any users from selling or transferring NFTs or any resources.

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::update_account_role(
    string owner_account_id_or_name,
    account_role_id_type role_id,
    optional<string> name,
    optional<string> metadata,
    flat_set<int> operations_to_add,
    flat_set<int> operations_to_remove,
    flat_set<account_id_type> accounts_to_add,
    flat_set<account_id_type> accounts_to_remove,
    optional<time_point_sec> valid_to,
    bool broadcast)
```
{% endcode %}



### 3.4. delete\_account\_role

This function deletes an account role.

{% hint style="warning" %}
Once an account role is deleted, restrictions on resource access no longer work!
{% endhint %}

{% code title="return type, namespace, & method" %}
```cpp
signed_transaction graphene::wallet::wallet_api::delete_account_role(
    string owner_account_id_or_name,
    account_role_id_type role_id,
    bool broadcast)
```
{% endcode %}



