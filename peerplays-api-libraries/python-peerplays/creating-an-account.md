# Creating an Account

There are two ways to create an account.

1. With Python-Peerplays: If you already have an account, it can be used to create another account.
2. With the Faucet: To create an account without an existing account.

### The Python-Peerplays Method

You need a funded account to create additional accounts. The funded account should be upgraded to create new accounts. To upgrade an account

```
p.upgrade_account(account="account_name")
```

Once the account is upgraded

`p.create_account(account_name="new_account_name", registrar="the_upgraded_account", owner_key='TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV', active_key='TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV', memo_key='TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV')`

### The Faucet Method

`url =` [`https://mint-faucet.peerplays.download/`](https://mint-faucet.peerplays.download/)``

```
params = {'account': {
    'name': 'new_account_name',
    'owner_key': 'TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV',
    'active_key': 'TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV',
    'memo_key': 'TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV',
    'refcode': '',
    'referrer': ''}}
```

`requests.post(url, json=params)`

## Additional Methods

```
p.transfer(to, amount, asset, memo="", account=None)
```
