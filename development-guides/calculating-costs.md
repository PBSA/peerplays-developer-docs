# Calculating Costs

## 1. Transaction Fee Considerations

Every operation on the Peerplays blockchain requires a small fee to cover the cost of the use of the network and data storage on the chain. These fees can add up so it's best to plan ahead, especially if you'll need to make hundreds or thousands of transactions.

The fees are set individually for every operation. The fees will also change over time due to blockchain governance voting. You can check the current fees using the **`get_global_properties`** function, and then calculate your total costs given how many times you'll need to run each operation.

You might do something like the following:

1. Use the `get_global_properties` function. Copy the whole list of operation IDs/fees from the return.
2. Download the latest copy of the [Operation ID mapping](../supporting-and-reference-docs/operation-ids-list.md#download).
3. Combine the two lists to get the fee for each operation.

Then you can use your combined list to easily calculate the total transaction costs for what you plan to do. At this time, there is no automated process for generating such a list.

{% hint style="info" %}
The PPY asset has a precision of 5. So the numbers listed in the `get_global_properties` function are in Satoshi. That is to say, to convert this number to nominal units, a decimal point is added 5 places from the right.

Some examples:

* `"fee": 1000` is really 0.01 PPY.
* `"premium_fee": 250000` is really 2.5 PPY.
* `"membership_lifetime_fee": 500000` is really 5 PPY.
{% endhint %}

### 1.1. get\_global\_properties

This function returns the blockchain’s slowly-changing settings (like the fees for each operation).&#x20;

This object contains all of the properties of the blockchain that are fixed or that change only once per maintenance interval (daily) such as the current list of witnesses, committee\_members, block interval, etc.

{% code title="return type, namespace, & method" %}
```cpp
global_property_object graphene::wallet::wallet_api::get_global_properties()const
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
Calling this function is simple:

{% code title="In the cli_wallet..." %}
```
get_global_properties
```
{% endcode %}
{% endtab %}

{% tab title="Return" %}
The `global_property_object` is returned:

```
get_global_properties
{
  "id": "2.0.0",
  "parameters": {
    "current_fees": {
      "parameters": [[
          0,{
            "fee": 1000,
            "price_per_kbyte": 1000
          }
        ],[
          1,{
            "fee": 50
          }
        ],[
          2,{
            "fee": 0
          }
        ],[
          3,{
            "fee": "500000000000"
          }
        ],[
          4,{}
        ],[
          5,{
            "basic_fee": 500,
            "premium_fee": 250000,
            "price_per_kbyte": 1000
          }
        ],[
          6,{
            "fee": 100,
            "price_per_kbyte": 1000
          }
        ],[
          7,{
            "fee": 1000
          }
        ],[
          8,{
            "membership_annual_fee": "500000000000",
            "membership_lifetime_fee": 500000
          }
        ]
		# ... #
      ],
      "scale": 10000
    },
    "block_interval": 3,
    "maintenance_interval": 3600,
    "maintenance_skip_slots": 3,
    "committee_proposal_review_period": 3600,
    "maximum_transaction_size": 99999,
    # ... #
    "extensions": {
      "betting_rake_fee_percentage": 100,
      "live_betting_delay_time": 0,
      "sweeps_distribution_percentage": 200,
      # ... #
    }
  },
  "next_available_vote_id": 118,
  "active_committee_members": [
    # ... #
  ],
  "active_witnesses": [
    # ... #
  ],
  "active_sons": [{
      # ... #
    }
  ]
}
```

{% hint style="info" %}
Parts of the example return have been truncated to fit. The actual return is much longer.
{% endhint %}
{% endtab %}
{% endtabs %}

### 1.2. An Example

Let's say you need to make 50,000 NFTs with custom permissions and issue them to various accounts.

To calculate the total cost for this, you'll find the transaction fees for each transaction to accomplish the goal and then multiply by how may times you need to execute each transaction.

Using `get_global_properties` and the list of operation ID's you find the following transaction fees:

1. Operation #85 -> `custom_account_authority_create_operation` -> Fee = 0.005 PPY & Kb of Data = 0.01 PPY
2. Operation #92 -> `nft_metadata_create_operation` -> Fee = 0.1 PPY & Kb of Data = 0.01 PPY
3. Operation #94 -> `nft_mint_operation` -> Fee = 0.01 PPY & Kb of Data = 0.01 PPY

You will need to create the custom permissions once: 0.005 PPY (the fee) + 0.01 PPY (for 1 Kb worth of data). Then you will need to create the NFT metadata once: 0.1 PPY (the fee) + 0.01 PPY (for 1 Kb worth of data). Last you will need to mint the 50,000 NFTs, which issues them to the accounts that will ultimately own them: 500 PPY (the fee) + 500 PPY (for 1 Kb worth of data).

{% hint style="info" %}
A Kb (Kilobyte) of data can accommodate about 1/2 page worth of text. So unless you have a lot of information to apply to your NFTs, the 0.01 PPY for a Kb of data should cover most use cases!
{% endhint %}

When added all together, you get:

0.005 + 0.01 + 0.1 + 0.01 + 500 + 500 = **1,000.125 PPY** cost in transaction fees to accomplish your goal. And this accounts for the use of an entire Kb of data per NFT. Using less data will make the total much less in this case.

This could represent 50,000 movie tickets, 50,000 shares of a company, or even 50,000 college degrees!

> Remember the 5 "P's"... Proper planning prevents poor performance!&#x20;
