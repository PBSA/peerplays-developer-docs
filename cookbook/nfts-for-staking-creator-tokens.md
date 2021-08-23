---
description: How communities can get work done. An example dapp idea using Peerplays.
---

# NFTs for Staking Creator Tokens

## 1. The Concept

Peerplays communities come in all shapes and sizes. Sometimes a community will need some specialized work to be done to accomplish the vision of the community. The Peerplays network covers some basic functions for everyone like, sidechain transfers, random number generation, asset exchanges, etc. But what if a community needs something else?

One example would be a community for book publishing. This community needs several specialized workers: file encoding, file storage, content verifiers, and Content Delivery Network \(CDN\) gateways to name a few. None of these are covered by Peerplays directly. But we can create a system which will incentivize community members to complete this work in a decentralized way.

## 2. The Plan

### 2.1. Components

This concept will make use of some Peerplays components:

* [Service Infrastructure Pools](https://community.peerplays.tech/technology/intro-to-peerplays-liquidity-pools/service-infrastructure-pools)
* [NFTs](https://devs.peerplays.tech/development-guides/nft-minting)
* [Custom Permissions](https://devs.peerplays.tech/development-guides/introduction-to-permissions)
* [User Issued Assets](https://devs.peerplays.tech/development-guides/creating-user-issued-assets)

This concept will also need some development for its custom components. These would be created by you, the dapp developer:

* Worker Node Software
* Decentralized User Interface

### 2.2. Putting it together

As the creator of the Peerplays community, you can use the components available to you to build a new system. The system can be simple or complex to best fit the needs of the community.

In this example system:

1. Community members enjoy the books available within and written by the community. The members purchase a specific community token \(LIBRARY, for example\) directly from the community. The profit from these sales is fed to the Service Infrastructure Pool.
2. The members then stake the LIBRARY tokens to receive community NFTs. The NFTs allow community governance voting and generate another community token \(BOOK, for example\) as rewards to the members over time.
3. Now a content creator \(an author\) needs the services of a community worker node. The author needs to pay the worker with BOOK token. So the author buys BOOK from the exchange and uses it to publish a book via the worker nodes.
4. BOOK is in high demand because it's also used by the members to check-out books for a limited time. So the workers can sell the BOOK in the exchange for profit.

The system so far is of a medium complexity and could satisfy the needs of the community. It's possible to add a little more to introduce some competition and buy-in among the worker nodes while also making the BOOK token deflationary.

* The workers can stake the BOOK token to receive a special worker NFT. This NFT provides the worker with "work power" which diminishes over time. The work power determines the amount of BOOK a worker is paid for performing a job as well as their priority for receiving jobs.

The Peerplays network makes many things possible. With imagination and will-power, you can create systems like this to realize your vision.

## 3. Related Documents

The components listed in this guide will cost transaction fees. To calculate how much PPY you'll need to make these transactions and meet your development goals, please see the [Calculating Costs](../development-guides/calculating-costs.md) guide.



