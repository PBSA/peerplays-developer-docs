---
title: NFT Store UI High Level Design
description: The high level design for the NFT Store user interface.
---

# NFT Store UI HLD

## 1. Introduction

NFTs \(Non-Fungible Tokens\) are quickly becoming an integral part of crypto-economies. With Peerplays aimed to become a foundational blockchain, NFTs need to be a large focus of its offerings. Additionally, Peerplays is uniquely suited to overcome many of the worst obstacles of NFT mass adoption: high transaction costs, slow network speeds, and limited storage capacity.

### 1.1. Project Goal

The overall goal of the Peerplays NFT Store project is to develop a user interface to enable users to easily perform NFT related operations on the Peerplays blockchain.

### 1.2. Purpose

The purpose of this High Level Design \(HLD\) document is to provide a concise and combined overview of the separate, more detailed, Low Level Design and Functional Specification documents to facilitate the sharing of information about this system. To find more information about the complete system design, see the Supporting & Reference Docs &gt; [NFT Store](https://devs.peerplays.tech/supporting-and-reference-docs/nft-development/nft-store) docs in the Peerplays Developer Documentation Portal.

## 2. NFT Store UI Site Map

![Figure 1. NFT Store UI Site Map](../../../.gitbook/assets/nft-store-map.png)

## 3. High-Level Process Flow Diagram

![Figure 2. High-Level Process Flow Diagram](../../../.gitbook/assets/nft-store-process-flow-diagram.png)

## 4. NFT Store Object Model

![Figure 3. NFT Store Object Hierarchy](../../../.gitbook/assets/nft-store-object-hierarchy.png)

### 4.1. Object Hierarchy

The NFT Store objects could be thought of like this:

* The NFT Store contains many `Creators`.
* A **`Creator`** can own many `Galleries`, `Collections`, and `NFTs`.
* A **`Gallery`** can contain many `Collections` and `NFTs`.
* A **`Collection`** can contain many `NFTs`. 

Figure 3 is a visual representation of the points above. The NFT Store is structured this way to provide a framework that is built with two important concepts at its core:

1. Consistency with industry standards, thereby meeting consumer expectations and instantly providing a familiar experience.
2. Giving Creators the flexibility to make unique, and branded, shopping experiences for their customers.

### 4.2. Creators

Creators are the enjoyers who come to make, buy, sell, and auction NFTs. They need a place to display their NFTs in a way that makes sense to potential buyers.

### 4.3. Galleries

Galleries can also be thought of as storefronts, showcases, shops, etc. These are meant to represent the highest level of branding and can provide a very customized, unique, and polished feel for customers. Like entering a store in a shopping mall or plaza, galleries can be used to contain the broadest categories of NFTs that still belong together.

### 4.4. Collections

Collections are essentially sets of related NFTs. Collections are meant for narrower categories so that the NFTs within are closely related.

### 4.5. NFTs

NFTs are the discrete units of sale. They may exist as one-offs that don't belong to any collection. More often they can relate closely together as a collection.

### 4.6. Some Examples

<table>
  <thead>
    <tr>
      <th style="text-align:left">Example</th>
      <th style="text-align:left">Creator</th>
      <th style="text-align:left">Gallery</th>
      <th style="text-align:left">Collection</th>
      <th style="text-align:left">NFT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>Clothing Shop in a Mall</p>
        <p>(Real life analogy)</p>
      </td>
      <td style="text-align:left">GAP</td>
      <td style="text-align:left">GAP Store#1125</td>
      <td style="text-align:left">Baby Clothes</td>
      <td style="text-align:left">Baby Boys Faded Jeans size 2T</td>
    </tr>
    <tr>
      <td style="text-align:left">NFT Store</td>
      <td style="text-align:left">Hiltos</td>
      <td style="text-align:left">Collectable Cards</td>
      <td style="text-align:left">Horse Race Card Game</td>
      <td style="text-align:left">Black Beauty first edition</td>
    </tr>
    <tr>
      <td style="text-align:left">NFT Store</td>
      <td style="text-align:left">Hiltos</td>
      <td style="text-align:left">Collectable Cards</td>
      <td style="text-align:left">Blockchain RPG Card Game</td>
      <td style="text-align:left">Longsword +2</td>
    </tr>
    <tr>
      <td style="text-align:left">NFT Store</td>
      <td style="text-align:left">Hiltos</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">My First NFT just for fun!</td>
    </tr>
  </tbody>
</table>

## 5. User Classifications

Several [user personas](https://devs.peerplays.tech/supporting-and-reference-docs/nft-development/nft-store/nft-store-user-stories#3-user-personas) have been identified to facilitate the development of the NFT Store design. While the personas demonstrate how people of various backgrounds may interact with the system, all users within the NFT Store will have the same features available to them. As such, only one type of user exists in the NFT Store, the NFT Store **creator**. They are named "creators" because the purpose of using the NFT store is to create.

## 6. System Security

The NFT Store server-side security will be accomplished through the use of SSL encryption and other server security best practices. Transactions on the blockchain are secured by public-private key encryption.

### 6.1. User Accounts

User accounts are issued and maintained on the blockchain through the [PeerID](https://devs.peerplays.tech/tools-and-integrations/peerid) system integration. Each creator must have a unique account registered in the Peerplays blockchain. Unauthenticated enjoyers can browse the NFT Store, but can't transact with the blockchain and are therefore required to log in to buy/sell/bid, create/edit NFTs, create galleries and collections, or manage their profile.

### 6.2. Passwords

Creator passwords are not stored on the network or PeerID system. Instead, the PeerID system can transact on behalf of the enjoyer using custom permission encrypted public-private key pairs and OAuth access tokens.

## 7. NFT Store Pages & Features

All NFT Store pages consist of:

* the site header
* the page body
* the site footer

The site header contains the Peerplays NFT Store identification using Peerplays branding guidelines. It also contains the main navigation menu, basic account functions, and a search bar.

The page body contains the content of the specific page.

The site footer contains marketing content, social media site links, broader scope of navigation, and links to legal documents such as privacy policy and terms of service, if required.

### 7.1. Accessing the NFT Store

#### 7.1.1. The NFT Store Homepage

The NFT Store homepage, or landing page, is the page at the root domain that enjoyers will first visit. This page is used to provide initial site navigation, marketing content, featured content, starting points for enjoyers, and helpful resources. The homepage can include a call to action, search bar, list of available categories, and the login function. The color scheme and graphical design of the homepage \(and **all** NFT store pages\) should align with Peerplays branding guidelines.

#### 7.1.2. Login / Logout

The login function will be managed with the PeerID system integration. Logging out invalidates the OAuth token thereby requiring another log in to access enjoyer data once again. The login / logout function should always be available to the enjoyer in the main nav menu.

#### 7.1.3. New Account Creation

The new account creation function will be managed with the PeerID system integration. Creating a new account should be available to unauthenticated enjoyers from the main nav menu.

### 7.2. User Profiles

#### 7.2.1. Logged In User's Profile

When logged in, an enjoyer can view and edit their own profile and settings.

Public profile fields the enjoyer can edit:

* Publicly displayed username
* Description / Bio \(free text field\)
* Profile pictures \(avatar, banner, background, etc.\)
* Social media profile links

Email notification settings include:

* When an item is sold
* When a bid is received on an item
* When a watched item price changes
* When an auction expires
* When you are outbid in an auction
* When the metadata changes on an owned NFT
* When you buy an item
* To receive the Peerplays newsletter

Display settings include:

* dark mode / light mode
* display language

#### 7.2.2. Other User's Profiles

It's also possible to view the profiles of other enjoyers. This displays their owned NFTs, NFT Collections, NFT Stores, account activity, public profile info, and activity relating to the logged in enjoyer \(if any\).

### 7.3 NFT Pages

#### 7.3.1. Creating a New NFT

#### 7.3.2. Editing an Existing NFT

#### 7.3.3. The Creator's Owned NFTs \(My NFTs\)

#### 7.3.4. NFTs On Offer \(for sale / auction\)

#### 7.3.5. NFT Details Page

### 7.4 Collection Pages

#### 7.4.1. Creating a New Collection

#### 7.4.2. Editing an Existing Collection

#### 7.4.3. The Creator's Collections \(My Collections\)

#### 7.4.4. Collection Details Page

### 7.5 Gallery Pages

#### 7.5.1. Creating a New Gallery

#### 7.5.2. Editing an Existing Gallery

#### 7.5.3. The Creator's Galleries \(My Galleries\)

#### 7.5.4. Gallery Details Page

## 8. Help and User Support

