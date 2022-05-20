---
description: >-
  The Peerplays NEX application (alpha) functional requirements specification
  for the market page.
---

# NEX-FS05a Market Page (alpha)

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the market page from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`page`

`process`

## 3. Scope

This FS will describe the requirements and basic design for the app’s market page. The market page should include all functions that are dedicated to the exchange of fungible assets as an open market.

### 3.1. Components

Specific components and features covered in this FS include:

* Market Page layout
* Order Book widget (market open orders)
* Trade History widget (market trade history)
* My Open Orders list (user open orders)
* My Trade History list (user trade/order fills history)
* Buy and Sell Controls
* Price and Volume Statistics
* Market Selector
* Balances Display
* Order Types
  * Market Orders
  * Limit Orders

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code        | Meaning                                   |
| ----------- | ----------------------------------------- |
| NEX-FS05a-# | NEX App Requirement (alpha) - Market Page |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

In a trading pair like `BTC/PPY` the first asset (`BTC`) is known as the `base` asset. The second asset (`PPY`) is known as the `quote` asset. The terms base and quote will be used throughout this FS.

## 5. Process Overviews

The processes which will be described here are as follows.

* Placing a Market Order
* Placing a Limit Order

### 5.1. Market Orders

To create a market order:

1. User uses the buy or sell form.
2. User enters the current market price (the Ask price if buying, the Bid price if selling) for the selected trading pair.
3. User enters the quantity of the asset they wish to buy or sell (ex. Quantity = 0.05 BTC).
4. The system displays to the user how much buying the asset will cost in terms of the selling asset and any applicable fees. (ex. buying 0.05 BTC, Fee = 0.00125 BTC, Total = 8.185 ETH)
5. User clicks the corresponding Buy or Sell button to initiate the market order.
6. The system displays a confirmation dialog to the user to ensure accuracy of the field inputs. This dialog also includes a password check as a reauthentication challenge to ensure the authenticity of the user.
7. User enters their master password and accepts the confirmation dialog.
8. The system clears the widget form fields.
9. The system initiates the market order on behalf of the user using the supplied field inputs.
10. The system notifies the user of the successful transaction.
11. The market order may be filled immediately, partially filled, or canceled (due to excessive slippage) depending on the current market conditions.
12. The order will appear in the order book and the open orders list. Any fulfilled trading (partial and/or full) will appear in the market and user trade history.

### 5.2. Limit Orders

To create a limit order:

1. User uses the buy or sell form.
2. User enters the quantity and limit price of the asset they wish to buy or sell (ex. BUY Quantity = 5 ETH, Limit Price = 0.05 BTC).
3. The system displays to the user how much buying the asset will cost in terms of the selling asset and any applicable fees. (ex. buying 5 ETH @ 0.05 BTC each, Fee = 0.00125 BTC, Total = 0.25125 BTC)
4. User clicks the corresponding Buy or Sell button to initiate the limit order.
5. The system displays a confirmation dialog to the user to ensure accuracy of the field inputs. This dialog also includes a password check as a reauthentication challenge to ensure the authenticity of the user.
6. User enters their master password and accepts the confirmation dialog.
7. The system clears the widget form fields.
8. The system initiates the limit order on behalf of the user using the supplied field inputs.
9. The system notifies the user of the successful transaction.
10. The limit order may be filled immediately, partially filled, or remain on the order book until fully filled or canceled depending on the price and quantity options specified.
11. The order will appear in the order book and the open orders list. Any fulfilled trading (partial and/or full) will appear in the market and user trade history.
12. The user may cancel any remaining portion of the order from the open orders list.

## 6. Context

The market page contains functions to trade assets in Peerplays. This page should contain everything a user needs to understand the current state of the market to make informed decisions about their trades.

## 7. Design Wire-frames

Designs for the NEX app market page are available here:

[DEX Hifi - All tabs with Market](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

Here’s a wire-frame of the current market page design:

![](<../../../../.gitbook/assets/NEX-FS05a Market Page (Current).png>)

## 8. Requirements

Requirements specific to the items listed in this FS are as follows.

### 8.1. Market Page Layout

The market page layout:

**NEX-FS05a-1:** shall be available for authenticated and unauthenticated users within the application menu.

**NEX-FS05a-2:** shall display the following components:

* market selector
* price and volume statistics
* order book
* market trade history
* buy order controls
* sell order controls
* user’s asset balances
* user’s open orders
* user’s order history (full and partial)

**NEX-FS05a-3:** shall use graphic design elements which adhere to Peerplays branding guidelines.

**NEX-FS05a-4:** shall use graphic design elements which remain consistent throughout the app.

**NEX-FS05a-5:** shall allow user input in relevant form fields to perform the functions of the related component.

**NEX-FS05a-6:** shall perform input field validation and inform the user of acceptable form inputs.

**NEX-FS05a-7:** shall provide unobtrusive help text to the user about each function. This may take any of the following forms:

* tooltip when hovering an element (button, title, etc.)
* pop-up when clicking an element (icon, link, etc.)
* expanding panels or menus with help text
* one time tutorial flow for new users
* similar techniques for revealing content

**NEX-FS05a-8:** shall indicate to the user when content is loading and avoid showing incorrect, outdated, or blank content.

### 8.2. Order Book widget (market open orders)

The order book widget:

**NEX-FS05a-9:** shall display a real-time aggregation of open buy and sell orders of the selected market pair.

**NEX-FS05a-10:** shall display the following information about each open order:

* price (in quote asset)
* amount (of base asset) available per price
* total cost (in quote asset) per amount (of base asset)

**NEX-FS05a-11:** shall allow the user to select various threshold (precision) levels to group orders by price. (This could be done by number of decimal places)

**NEX-FS05a-12:** shall display the market spread (gap in price between ask price and bid price.)

**NEX-FS05a-13:** shall allow the user to select to view buy orders only, sell orders only, or both buy and sell orders.

**NEX-FS05a-14:** shall allow the user to click a price level to apply that data to the buy and sell controls automatically.

**NEX-FS05a-15:** the design shall indicate to the user buy orders (green) vs sell orders (red) vs spread (neutral).

### 8.3. Trade History widget (market trade history)

The trade history widget:

**NEX-FS05a-16:** shall display all recently executed market taker trades of the selected market pair arranged in a list chronologically from most recent.

* number of trades displayed may be based on time (past 12 hours)
* number of trades displayed may be based on number of trades (past 100 trades)

**NEX-FS05a-17:** shall be updated in real-time.

**NEX-FS05a-18:** shall include the trade quantity (base asset), price (quote asset), and timestamp.

**NEX-FS05a-19:** the UI shall indicate if each market taker trade was a buy (green) or a sell (red) trade. The design should indicate a buy or sell trade with icons, arrows, or other design elements as well as color to help facilitate universal design implementation.

### 8.4. My Open Orders list (user open orders)

The my open order list:

**NEX-FS05a-20:** shall include all of the user's currently open orders (regardless of selected market pair) with real-time data updates.

* should allow the user to toggle viewing open orders for the selected market pair vs all their open orders.

**NEX-FS05a-21:** shall include the following data fields:

* the date and time each order was opened.
* the market pair
* if the order is a buy or sell ("side")
* the order type (limit)
* the order price (quote asset)
* the total order quantity (base asset)
* the filled order quantity (base asset)
* the total price (quote asset)
* the incurred fees
* the order status (open or partially filled)
* may display a separate list of filled quantities of a partially filled order.

**NEX-FS05a-22:** shall allow the user to sort the list by its data fields.

**NEX-FS05a-23:** shall allow the user to filter the list by its data fields.

**NEX-FS05a-24:** shall allow the user to cancel any open order, even if partially filled.

* user control for this may be a button or context menu item.

### 8.5. My Trade History list (user trade history)

The my trade history list:

**NEX-FS05a-25:** shall include all of the user's completed trade activities (regardless of selected market pair) with real-time data updates.

* may allow the user to toggle viewing their trade history for the selected market pair vs all their trade history.

**NEX-FS05a-26:** shall include the following data fields:

* the date and time each order was opened.
* the date and time each trade was completed (filled, partially filled, canceled).
* the market pair
* if the order was a buy or sell ("side")
* the order type (market or limit)
* the trade price (quote asset)
* the filled order quantity (base asset)
* the total price (quote asset)
* the incurred fees
* the order status (partially filled, filled, canceled by user, canceled by system)
* may display a separate list of filled quantities of a partially filled order.

**NEX-FS05a-27:** shall allow the user to sort the list by its data fields.

**NEX-FS05a-28:** shall allow the user to filter the list by its data fields.

### 8.6. Buy / Sell controls

The buy / sell controls:

**NEX-FS05a-29:** shall allow the user to select between buy and sell orders by supplying the appropriate forms.

**NEX-FS05a-30:** shall allow the user to select between market and limit order types.

**NEX-FS05a-31:** shall allow the user to input the order amount (quantity of base asset).

**NEX-FS05a-32:** shall, if the order type is a limit order, allow the user to input the limit price (quote asset).

**NEX-FS05a-33:** shall display to the user the fees of the order, given the user’s inputs.

**NEX-FS05a-34:** shall display to the user the total (quote asset) of the order, given the user’s inputs.

**NEX-FS05a-35:** shall allow the user to create an order if the following criteria is met:

* the user is logged in.
* the order form is filled in with (successfully) validated inputs.
* the user has an appropriate amount of assets to satisfy the needs of the order.

**NEX-FS05a-36:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS05a-37:** shall provide user controls (buttons) to initiate buy and sell orders.

**NEX-FS05a-38:** shall, once initiated by the user, summarize the order and ask the user for their confirmation to proceed.

**NEX-FS05a-39:** shall, once initiated or confirmed by the user, indicate order is in process. (loading spinner)

**NEX-FS05a-40:** shall, once confirmed by the user, submit the order to the Peerplays network.

**NEX-FS05a-41:** shall indicate to the user if the order was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS05a-42:** shall, if the order initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS05a-43:** shall, upon successful order initiation, add the order to the order book, update the user’s asset balances, add the order to the user’s open orders (for limit orders), etc.

### 8.7. Price / Volume statistics

The price / volume statistics:

**NEX-FS05a-44:** shall display the last price (quote asset) for the selected market pair.

**NEX-FS05a-45:** shall display the Bid and Ask prices (quote asset).

**NEX-FS05a-46:** shall display the 24-hour price change in terms of raw value and in percentage (quote asset).

**NEX-FS05a-47:** shall display the 24-hour price range (high and low, in quote asset).

**NEX-FS05a-48:** shall update the price and volume statistics in real-time based on current market conditions.

### 8.8. Market Selector

The market selector:

**NEX-FS05a-49:** shall display the currently selected market (trading pair).

**NEX-FS05a-50:** shall allow the user to select any available market.

**NEX-FS05a-51:** shall allow the user to flip the base and quote assets of the trading pair. For example, users need to be able to change the selected market from BTC-ETH to ETH-BTC.

**NEX-FS05a-52:** shall, when a new market is selected by the user, update the page to reflect the selected market. (order book, history, asset balances, prices, etc.)

### 8.9. Balances Display

The balances display:

**NEX-FS05a-53:** shall display the user's asset balances available for trading relating to the currently selected market.

**NEX-FS05a-54:** shall update the available balances in the display when orders are created, filled, partially filled, or canceled.

## 9. Appendix A: Glossary

| Term         | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RS           | Requirements Specification                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| FS           | Functional Specification                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| UI           | User Interface                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Market Order | Orders that are meant to execute as quickly as possible at the current market price. If you’re buying an asset, a market order will execute at whatever price the seller is asking. If you’re selling, a market order will execute at whatever the buyer is bidding. The biggest drawback of the market order is that you can’t specify the price of the trade. Market orders cannot be canceled because they are filled immediately. Market orders may be partially filled at several prices. |
| Limit Order  | An order that sets the maximum or minimum price at which you are willing to buy or sell. The biggest advantage of the limit order is that you get to name your price, and if the asset reaches that price, the order will be filled.                                                                                                                                                                                                                                                           |
