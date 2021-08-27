# comparison-between-scenarios-for-handling-deposits-and-withdrawals

## Comparison between scenarios for handling deposits and withdrawals

## Intro

The purpose of deposit handling is to detect cryptocurrency payment from Peerplays user \(currently only Bitcoin\), and issue matching amount of Peerplays assets \(pBTC\) on Peerplays network, which user can use in games.

The purpose of withdrawal handling is to do the same thing in reverse - user will initiate withdrawal process, system will exchange his Perplays assets \(pBTC\) for the matching amount of cryptocurrency, and send this amount to user’s account.

In this document, I will describe scenarios we are considering, and present the main differences between them. There are features common to all scenarios \(like user address mappings and sidechain manager features, verifying transaction validity reported by different SONs...\), and I will consider them to be out of the scope of this document.

## Scenarios

At this moment we are considering three possible approaches:

* S1: Multisignature primary wallet controlled by SON network, holding all the funds. All transfers are made to and from this multisignature wallet.
* S2: Proxy account, created by Peerplays network, paired with multisignature primary wallet controlled by SON network. On deposit, user sends funds to proxy account and funds are moved from proxy account to a primary wallet. On withdrawal, user initiates withdrawal process and funds are moved from primary wallet to proxy account or to another account belonging to the user. There is no clear suggestion on the approach. **This is existing sidechain design, implemented in feature\_sidechain branch.**
* S3: 3 of 3 multisig wallet, controlled by blockchain network, SON network and user, paired with temporary primary wallet controlled by SON network. On deposit, user sends funds to 3 of 3 wallet, funds are staying in 3 of 3 wallet, wallets are periodically rebalanced to match the balance between 3 of 3 wallet and Peerplays pBTC, and funds are moving between 3 of 3 accounts and temporary primary wallet. On withdrawal, user initiates withdrawal process, selects one of three withdrawal scenarios, and once the withdrawal is executed, funds will be moved from temporary primary wallet to 3 of 3 wallet or to another account belonging to the user. There is no clear suggestion on the approach. **This is Jonathans proposal.**

## General description of deposit process

* User send’s cryptocurrency to some address controlled by Peerplays. Depending on what's behind this address, we will need more or less work for implementation and maintaining it during system usage. Current suggestions are:
  * S1: User sends funds to a primary wallet, holding all the funds, and controlled by SON network.
  * S2:  User sends funds to a user’s single signature proxy account, created by Peerplays network. Network will pull the funds from proxy account to primary wallet controlled by SON network, when applicable.
  * S3:  User sends funds to a 3 of 3 wallet, where signers are Blockchain network, SON network and user.
* SON network monitors this address for transfers, and when transfer is detected, it will initiate a process of issuing pBTC.
  * S1: SON network monitors address of primary wallet. When transfer is detected, the system will identify user who made the payment and issue pBTC to user’s Peerplays address.
  * S2: SON network monitors address of users’s single signature proxy account. When transfer is detected, funds will be moved from proxy account to primary wallet, system will identify user who made the payment, and issue pBTC to user’s Peerplays address.
  * S3:  SON network monitors address of 3 of 3 wallet. When transfer is detected, funds will stay locked in 3 of 3 wallet, system will identify user who made the payment, and issue pBTC to user’s Peerplays address.
* Once the pBTC is issued, user will not be able to freely handle the crypto-currency he initially paid. Current suggestions are:
  * S1: Funds are moved to Primary wallet controlled by network.
  * S2: Funds are moved from proxy account to Primary wallet account
  * S3: Funds stay in a 3 of 3 wallet, and in order to move them, three signatures are required

## General description of withdrawal process

* User initiate withdrawal process. Depending on how the user will initiate withdrawal process we we will need more or less work for implementation and maintaining it during system usage. Current suggestions are:
  * S1: No clear suggestion on this. However, it can be started by initiating transfer from user’s peerplays account to a primary wallet controlled by SON network in Peerplays network.
  * S2: No clear suggestion on this. However, it can be started by initiating transfer from user’s peerplays account to a primary wallet controlled by SON network in Peerplays network.
  * S3: User can select one of three withdrawal methods, instant, standard and econo. The difference between these three is the price the user will pay for withdrawal, and the amount of time needed for withdrawal to be processed. If user pays higher price, withdrawal will be processed faster.
* When SON network receive withdrawal process request, it needs to identify user who initiated a request, and send funds to his account. In current scenario, we only handle BTC. In the future, when we support more blockchain networks, we also need to identify the network where we will send crypto-currency \(e.g. if user made his paymetn from BTC and ETH network, we can allow him to choose whether he will be paid in BTC or ETH\). However, we will also need a way to prevent user to use Peerplays network as an exchange office, in order to speculate with exchange rates, and pull more funds from the system, without actually using the Peerplays network for gaming. 
  * S1: No clear suggestion on this, but we can send funds to a user account on target sidechain registered in the system.
  * S2: No clear suggestion on this, but we can send funds to a user account on target sidechain registered in the system, or to a user’s proxy account used for initial payment.
  * S3: Funds will be sent to 3 of 3 wallet during rebalance. However, user still needs 3 signatures to withdraw his funds from 3 of 3 to his personal account. There is no clear suggestion on how this will be done.

## Problems on active SONs set change

Various problems may occur when we are using multi signature wallets controlled by SONs network, and active SONs set changes. Multisignature wallet is not updateable - we can’t update the list of signatures required for using funds from multisignature wallet. When active SONs set changes, multisignature wallets created by previous active SONs set may become unusable \(because previous SONs holding ⅔ weight are not available anymore\), rendering funds unavailable. To prevent this, the general approach is to recreate the multi signature wallets, and move all the funds from the old one to the new one. For some blockchain, like Ethereum, this problem may be easily solved by using smart contracts, and no recreation will be needed. However, since our current target is Bitcoin, the app

* S1: Primary wallet needs to be recreated and funds moved from old wallet to the new one.
* S2: Primary wallet needs to be recreated and funds moved from old wallet to the new one.
* S3: All 3 of 3 wallets, and temporary primary wallet needs to be recreated and funds moved from old wallets to the new ones. User interaction with the system might be needed for some of these actions.

## Rebalancing

* S1: Rebalancing is not needed.
* S2: Rebalancing is not needed.
* S3: All 3 of 3 are periodically rebalanced to match the balance of pBTC in Peerplays network. Rebalance can be optimized by addressing only accounts that actually needs rebalancing, and transferring only differences that are needed to match the BTC and pBTC balances.

## Comparison

* S1: This design is a kind of “lowest common denominator” approach, that will offer functional system, with lowest costs \(single transaction for deposit, and single transaction for withdrawal\), it is easiest to implement and most flexible to add changes later. It is easier to convert S1 into S2 or S3 than S2 into S3 or S3 into S2, or any other way around. The main problem with this design is PW recreation and moving funds from old to the new one, and the fact that PW will hold all Bitcoins in the network, which can make him target for hackers in the future, when it may hold enough value for somebody to go for it.
* S2 is very similar to S1 in all terms, but it is not clear why do we need the proxy account. It increases cost \(one transaction for user account to proxy and one transaction for proxy to PW for deposit, and one transaction for PW to proxy and one transaction for proxy to user account for withdrawal\). The main problem with this design is PW recreation and moving funds from old to the new one, and the fact that PW will hold all Bitcoins in the network, which can make him target for hackers in the future, when it may hold enough value for somebody to go for it. Another problem is increased costs of system usage, because of using proxy accounts.
* S3: By far the most complex design, advertised as “no primary wallet needed”, but from the discussion we had, we actually do need it for rebalancing and for holding the surplus of assets that are not distributed to 3 of 3 wallets during rebalancing. Temporary or not, multisig wallet controlled by SON network is there, and it needs handling. User interactions with the system in order to allow pulling funds from 3 of 3 wallet to temporary primary wallet \(for handling "half signed" PUAT and PUST transactions\) is problematic from the point of getting funds earned by Peerplays network, as user does not have clear motivation to sign any of these \(e.g. user’s pBTCs are all spent, so why would user care did Peerplays pulled the BTC from his account or not\). The good side of this design is that PW will not hold all assets, only part of them, while the other part will be distributed in user’s 3 of 3 wallets.

## Recommendation

In order to unblock development, I recommend that we go with S1, since it will enable us to get the functionality we need with the least effort and shortest time. With S1 as a starting point, we will implement all common features for all three designs \(son communication, multisig wallet recreation, moving funds, signing transactions, user address mappings, sidechain manager functionalities, etc…\) and once we have S1 done, we can tweak it to any other scenario.

