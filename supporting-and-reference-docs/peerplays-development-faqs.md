# Peerplays Development FAQs

## Bitcoin-SON

### ❓ How can I generate Bitcoin addresses?

Command:

```text
./bitcoin-cli -rpcuser=1 -rpcpassword=1 -rpcwallet=default getnewaddress
```

References:

{% embed url="https://chainquery.com/bitcoin-cli/getnewaddress" %}

{% embed url="https://developer.bitcoin.org/reference/rpc/getnewaddress.html" %}

### ❓ How to get TEST BTC on the REGTEST network?

From developer.bitcoin.org...

> For situations where interaction with random peers and blocks is unnecessary or unwanted, Bitcoin Core’s regression test mode \(regtest mode\) lets you instantly create a brand-new private block chain with the same basic rules as testnet—but one major difference: you choose when to create new blocks, so you have complete control over the environment.
>
> Many developers consider regtest mode the preferred way to develop new applications. The following example will let you create a regtest environment after you first [configure bitcoind](https://developer.bitcoin.org/examples/index.html).

```text
bitcoind -regtest -daemon
```

> Start `bitcoind` in regtest mode to create a private block chain.

```text
bitcoin-cli -regtest generatetoaddress 101 $(bitcoin-cli -regtest getnewaddress)
```

> Generate 101 blocks using a special [RPC](https://developer.bitcoin.org/reference/rpc/index.html) which is only available in regtest mode. This takes less than a second on a generic PC. Because this is a new block chain using Bitcoin’s default rules, the first blocks pay a block reward of 50 bitcoins. Unlike mainnet, in regtest mode only the first 150 blocks pay a reward of 50 bitcoins. However, a block must have 100 confirmations before that reward can be spent, so we generate 101 blocks to get access to the coinbase transaction from block \#1.

```text
bitcoin-cli -regtest getbalance
```

> Verify that we now have 50 bitcoins available to spend.
>
> You can now use Bitcoin Core [RPCs](https://developer.bitcoin.org/reference/rpc/index.html) prefixed with `bitcoin-cli -regtest`.
>
> Regtest wallets and block chain state \(chainstate\) are saved in the `regtest` subdirectory of the Bitcoin Core configuration directory. You can safely delete the `regtest` subdirectory and restart Bitcoin Core to start a new regtest. \(See the [Developer Examples Introduction](https://developer.bitcoin.org/examples/index.html) for default configuration directory locations on various operating systems. Always back up mainnet wallets before performing dangerous operations such as deleting.\)

Reference:

{% embed url="https://developer.bitcoin.org/examples/testing.html\#regtest-mode" %}

### ❓ How can I generate / find the public key for a Bitcoin address?

Command:

```text
./bitcoin-cli -rpcuser=1 -rpcpassword=1 -rpcwallet=default getaddressinfo "address"
```

References:

{% embed url="https://chainquery.com/bitcoin-cli/getaddressinfo" %}

{% embed url="https://developer.bitcoin.org/reference/rpc/getaddressinfo.html" %}

### ❓ If funds are deposited to a Bitcoin address, will there be logs?



### ❓ What is the cli\_command to generate a Bitcoin deposit address in the Peerplays blockchain?



### ❓ If a Bitcoin deposit address is created for a Peerplays address \(account\) what are the logs in the Peerplays blockchain?



### ❓ How to run a Peerplays blockchain node with RPC debugging turned on?



## Gamified Proof of Stake \(GPOS\)

### ❓ What are the blockchain logs when a successful GPOS Vesting is done?



## Faucet

### ❓ What are the logs in faucet when an account is successfully created?

One can use **Curl** command or **POSTMAN** in order to create a new account in the Faucet. With **Curl** you can do the following:

```text
curl -X POST http://<IP_ADDRESS>:<PORT_NUMBER>/api/v1/accounts 
-H 'content-type: application/json' 
-d '{
	"account": {
		"name": "<ACCOUNT_NAME>",
		"owner_key": "TEST5WaszCsqVN9hDkXZPMyiUib3dyrEA4yd5kSMgu28Wz47B3wUqa",
		"active_key": "TEST5TPTziKkLexhVKsQKtSpo4bAv5RnB8oXcG4sMHEwCcTf3r7dqE",
		"memo_key": "TEST5TPTziKkLexhVKsQKtSpo4bAv5RnB8oXcG4sMHEwCcTf3r7dqE"
	}
}'
```

{% hint style="info" %}
**IP\_ADDRESS** should be **127.0.0.1** or **localhost** when running it directly in a VM or on your local machine.

**`owner_key`**, **`active_key`**, **`memo_key`** should start with **TEST** on testnet, and it should be **PPY** on mainnet.
{% endhint %}

If the result of the above is successful, there should be output like this:

```text
{
	"account": {
		"active_key": "TEST5TPTziKkLexhVKsQKtSpo4bAv5RnB8oXcG4sMHEwCcTf3r7dqE",
		"memo_key": "TEST5TPTziKkLexhVKsQKtSpo4bAv5RnB8oXcG4sMHEwCcTf3r7dqE",
		"name": "account-name4",
		"owner_key": "TEST5WaszCsqVN9hDkXZPMyiUib3dyrEA4yd5kSMgu28Wz47B3wUqa",
		"referrer": "nathan"
	}
}
```

And in case of any error, it will be following \(i.e. duplicate account\):

```text
{"error":{"base":["Account exists"]}}
```

You can also track the logs in **Faucet** container, with the following command:

```text
docker logs --follow <Faucet-Container-ID>
For example: docker logs --follow 47c101add138
```

And look for logs when receiving **Create Account** requests, with **200** status code as a result.

```text
37.252.95.183 - - [06/Oct/2021 17:55:29] "OPTIONS /api/v1/accounts HTTP/1.1" 200 -
37.252.95.183 - - [06/Oct/2021 17:55:30] "POST /api/v1/accounts HTTP/1.1" 200 -
```

