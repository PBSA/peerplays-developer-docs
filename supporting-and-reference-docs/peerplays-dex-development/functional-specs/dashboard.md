---
description: Dex Functional Specifications - Dashboard
---

# Dashboard

## 1. Purpose

The purpose of this document is to outline functional specifications for the Peerplays Decentralized Exchange \(Dex\) relating to the Dashboard page from the user's perspective.

## 2. Scope

The Dex will be a feature rich application that users will depend on to trade their native and sidechain assets. As development on the Dex continues the list of features available to users will grow. A Dashboard page offers users the ability to see their account activity at-a-glance as well as provide the essential actions users often take. In this way, the Dashboard provides the user the basics of the Dex on one page.

Specific functions covered include:

* the Dashboard page
* the Quick Order widget
* the Quick Send widget
* the My Activity list
* the My Assets list

## 3. Process Overview

Two processes will be described here: using the Quick Order widget, and using the Quick Send widget.

### 3.1. Quick Order widget

**Goal:** The Quick Order widget must allow the following actions:

* The user can quickly place a market order to sell an asset they own at the current market price.
* The user can quickly place a market order to buy an asset at the current market price.

To create a buy market order:

1. User selects the buy tab in the Quick Order widget.
2. User selects the asset they wish to buy \(ex. BTC\).
3. User selects the asset with which they will pay \(ex. ETH\).
4. User enters the quantity of the asset they wish to buy \(ex. 0.5\).
5. The system displays to the user how much buying the asset will cost in terms of the selling asset. \(ex. Buying 0.5BTC will cost 20ETH at current market rates.\)
6. User clicks Place Order to initiate the buy market order.
7. The system displays a confirmation dialog to the user to ensure accuracy of the field inputs.
8. User accepts the confirmation dialog.
9. The system clears the widget form fields.
10. The system initiates the buy market order on behalf of the user using the supplied field inputs.
11. The system notifies the user of the successful transaction.

To create a sell market order:

1. User selects the sell tab in the Quick Order widget.
2. User selects the asset they wish to sell \(ex. BTC\).
3. User selects the asset for which they will be paid \(ex. ETH\).
4. User enters the quantity of the asset they wish to sell \(ex. 0.5\).
5. The system displays to the user how much selling the asset will earn them in terms of the earned asset. \(ex. Selling 0.5BTC will receive 20ETH at current market rates.\)
6. User clicks Place Order to initiate the sell market order.
7. The system displays a confirmation dialog to the user to ensure accuracy of the field inputs.
8. User accepts the confirmation dialog.
9. The system clears the widget form fields.
10. The system initiates the sell market order on behalf of the user using the supplied field inputs.
11. The system notifies the user of the successful transaction.

### 3.2. Quick Send widget

To send an asset with the Quick Send widget:

1. User enters an account name \(with which they have permissions to transfer assets from\) in the From field.
2. User enters an account name in the To field.
3. User enters a quantity into the Quantity field.
4. User selects an asset in the asset selector dropdown.
5. The system displays the Fee required to initiate the send to the user.
6. User \(optionally\) adds a memo into the Memo field.
7. User clicks the Send button.
8. The system displays a confirmation dialog to the user to ensure accuracy of the field inputs.
9. User accepts the confirmation dialog.
10. The system clears the widget form fields.
11. The system initiates the transfer on behalf of the user using the supplied field inputs.
12. The system notifies the user of the successful transaction.

## 4. Context

The Dashboard provides user-friendliness to the Dex by providing a simplified way of interacting with all the various components of the Dex. The Dashboard is intended to be accessible and concise. While the Dashboard is wide in the scope of its offerings, it has a minimal depth of features.

## 5. Requirements

Requirements specific to the items outlined in this functional specification are as follows.

### 5.1. Dashboard page

The dashboard page:

* must be available for authenticated users within the application menu.
* must display widgets and components that have been configured to be active in the Dashboard.

The system:

* must not allow unauthenticated users to view the Dashboard.

### 5.2. Quick order widget

The quick order widget:

* must allow the creation of buy and sell market orders.
* must display the fee for placing an order.
* must provide the following actions via buttons.
  * Place Order

For buy market orders the widget:

* must allow the user to select a Buy tab or toggle to indicate the user is placing a buy market order.
* must provide the following fields.
  * Asset to buy \(required pre-populated dropdown of available assets\)
  * Quantity to buy \(required field\)
  * Asset to pay \(required pre-populated dropdown of owned assets\)
* must provide real-time field validation as per the following.
  * Required fields are complete.
  * Quantity field is within relevant limits.

For sell market orders the widget:

* must allow the user to select a Sell tab or toggle to indicate the user is placing a sell market order.
* must provide the following fields.
  * Asset to sell \(required pre-populated dropdown of owned assets\)
  * Quantity to sell \(required field\)
  * Asset to receive \(required pre-populated dropdown of available assets\)
* must provide real-time field validation as per the following.
  * Required fields are complete.
  * Quantity field is within relevant limits.

Upon clicking Place Order, the system:

* must validate the form input.
* must display a confirmation dialog that provides the user with the following actions via buttons.
  * Confirm
  * Cancel
* the confirmation dialog must display relevant information to the user about the transaction such as:
  * The form inputs
  * The up to date market values of the order.
  * The fee to perform the transaction
  * may include remaining funds after transfer
* Upon cancel, the system must close the dialog and wait for the user to act again.
* Upon confirm:
  * the system must create and broadcast an order on behalf of the user using the validated form input.
  * The system must clear the form fields.
  * Upon success, the system must indicate to the user that the order was successful.

### 5.3. Quick send widget

The quick send widget:

* must provide the following fields.
  * From \(required field\)
  * To \(required field\)
  * Quantity \(required field\)
  * Asset \(required dropdown\)
  * Memo \(optional field\)
* must provide the following information to the user.
  * fee amount for the transaction
  * a note that only users with a memo key can read memos
* must provide the following actions via buttons.
  * Send
* must provide real-time field validation as per the following.
  * Required fields are complete.
  * From field is populated with an account for which the user has permissions to transfer funds from.
  * To field is populated with an account that exists.
  * The asset selector dropdown is only populated with assets the user owns.
  * The quantity field must be within the relevant limits. \(minimum send limit, maximum of available quantity\)
  * The account must be able to accommodate the fee.

When the send button is clicked, the system:

* must validate the form input.
* must display a confirmation dialog that provides the user with the following actions via buttons.
  * Confirm
  * Cancel
* the confirmation dialog must display relevant information to the user about the transaction such as:
  * The form inputs
  * The fee to perform the transaction
  * may include remaining funds after transfer
* Upon cancel, the system must close the dialog and wait for the user to act again.
* Upon confirm:
  * the system must create and broadcast a transfer on behalf of the user using the validated form input.
  * The system must clear the form fields.
  * Upon success, the system must indicate to the user that the transfer was successful.

### 5.4. My activity list

The my activity list:

* must include all recent transactions regarding the authenticated account.
* must include the following columns of information.
  * Time
  * Transaction Type
  * Transaction Description \(Info\)
  * Transaction ID
  * Fee

### 5.5. My assets list

The my assets list:

* must include all assets regarding the authenticated account.
* must include the following columns of information.
  * Asset
  * Available Balance
  * Price in BTC
  * Change \(24h\)
  * Value in BTC

### 5.6. The system

* if an error occurs at any point \(Quick Order widget, Quick Send widget\), the system must display meaningful error information to the user and provide them with actions they can take to attempt to resolve the error.

## 6. Glossary

**Market order**​ : Orders that are meant to execute as quickly as possible at the current market price. If you’re buying an asset, a market order will execute at whatever price the seller is asking. If you’re selling, a market order will execute at whatever the buyer is bidding. The biggest drawback of the market order is that you can’t specify the price of the trade.

**Limit order** : An order that sets the maximum or minimum price at which you are willing to buy or sell. The biggest advantage of the limit order is that you get to name your price, and if the asset reaches that price, the order will be filled. If the asset never reaches the limit price, the trade won’t execute.

