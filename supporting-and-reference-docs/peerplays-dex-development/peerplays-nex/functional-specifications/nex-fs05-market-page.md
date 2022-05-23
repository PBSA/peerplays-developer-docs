---
description: >-
  The Peerplays NEX application functional requirements specification for the
  market page.
---

# NEX-FS05 Market Page

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
* Price Chart
* Market Depth Chart
* Price and Volume Statistics
* Market Selector
* User Wallet Functions
* Balances Display
* Order Types (and Time in Force Policies)
  * Market Orders
  * Limit Orders
    * Good Til Canceled
    * Good Til Time
    * Immediate-or-Cancel
    * Fill-or-Kill
    * Marker-or-Cancel
  * Stop-Limit Orders

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                           |
| ---------- | --------------------------------- |
| NEX-FS05-# | NEX App Requirement - Market Page |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

In a trading pair like `BTC/PPY` the first asset (`BTC`) is known as the `base` asset. The second asset (`PPY`) is known as the `quote` asset. The terms base and quote will be used throughout this FS.

## 5. Process Overviews

The processes which will be described here are as follows.

* Placing a Market Order
* Placing a Limit Order
* Placing a Stop-Limit Order

### 5.1. Market Orders

To create a market order:

1. User selects buy or sell.
2. User selects the market tab in the buy / sell controls.
3. User enters the quantity of the asset they wish to buy or sell (ex. Quantity = 0.05 BTC).
4. The system displays to the user how much buying the asset will cost in terms of the selling asset and any applicable fees. (ex. buying 0.05 BTC, Fee = 0.00125 BTC, Total = 8.185 ETH)
5. User clicks “Place Order” button to initiate the market order.
6. The system displays a confirmation dialog to the user to ensure accuracy of the field inputs. This dialog also includes a password check as a reauthentication challenge to ensure the authenticity of the user.
7. User enters their master password and accepts the confirmation dialog.
8. The system clears the widget form fields.
9. The system initiates the market order on behalf of the user using the supplied field inputs.
10. The system notifies the user of the successful transaction.
11. The market order may be filled immediately, partially filled, or remain on the order book until filled or canceled. (See [section 9. Appendix A: Glossary](https://devs.peerplays.tech/supporting-and-reference-docs/peerplays-dex-development/peerplays-nex/functional-specifications/nex-fs05-market-page#9.-appendix-a-glossary) for more details on the order types.)
12. The order will appear in the order book and the open orders list. Any fulfilled trading (partial and/or full) will appear in the market and user trade history.
13. The user may cancel any remaining portion of the order from the open orders list.

### 5.2. Limit Orders

To create a limit order:

1. User selects buy or sell.
2. User selects the limit tab in the buy / sell controls.
3. User enters the quantity and limit price of the asset they wish to buy or sell (ex. BUY Quantity = 5 ETH, Limit Price = 0.05 BTC).
4. User specifies any advanced options for the limit order (limit order type) as follows:
   1. Good Til Canceled - No additional options.
   2. Good Til Time - User selects a "Cancel after" time-frame (1m, 5m, 15m, 1h, 6h, 1d, 1wk).
   3. Immediate-or-Cancel - No additional options.
   4. Fill-or-Kill - No additional options.
   5. Maker-or-Cancel - No additional options.
5. The system displays to the user how much buying the asset will cost in terms of the selling asset and any applicable fees. (ex. buying 5 ETH @ 0.05 BTC each, Fee = 0.00125 BTC, Total = 0.25125 BTC)
6. User clicks “Place Order” button to initiate the limit order.
7. The system displays a confirmation dialog to the user to ensure accuracy of the field inputs. This dialog also includes a password check as a reauthentication challenge to ensure the authenticity of the user.
8. User enters their master password and accepts the confirmation dialog.
9. The system clears the widget form fields.
10. The system initiates the limit order on behalf of the user using the supplied field inputs.
11. The system notifies the user of the successful transaction.
12. The limit order may be filled immediately, partially filled, canceled, or remain on the order book until filled, canceled, or timed out depending on the advanced options specified. (See [section 9. Appendix A: Glossary](https://devs.peerplays.tech/supporting-and-reference-docs/peerplays-dex-development/peerplays-nex/functional-specifications/nex-fs05-market-page#9.-appendix-a-glossary) for more details on the order types.)
13. The order will appear in the order book and the open orders list. Any fulfilled trading (partial and/or full) will appear in the market and user trade history.
14. The user may cancel any remaining portion of the order from the open orders list.

### 5.3. Stop-Limit Orders

To create a stop-limit order:

1. User selects buy or sell.
2. User selects the stop-limit tab in the buy / sell controls.
3. User enters the stop price of the asset they wish to buy or sell.
4. User enters the quantity and limit price of the asset they wish to buy or sell (ex. BUY Stop Price = 0.04 BTC, Quantity = 5 ETH, Limit Price = 0.05 BTC).
5. The system displays to the user how much buying the asset will cost in terms of the selling asset and any applicable fees. (ex. buying 5 ETH @ 0.05 BTC each, Fee = 0.00125 BTC, Total = 0.25125 BTC)
6. User clicks “Place Order” button to initiate the stop-limit order.
7. The system displays a confirmation dialog to the user to ensure accuracy of the field inputs. This dialog also includes a password check as a reauthentication challenge to ensure the authenticity of the user.
8. User enters their master password and accepts the confirmation dialog.
9. The system clears the widget form fields.
10. The system initiates the stop-limit order on behalf of the user using the supplied field inputs.
11. The system notifies the user of the successful transaction.
12. The stop-limit order is triggered only when the stop price is crossed. At that point, a limit order will be created. (See [section 9. Appendix A: Glossary](https://devs.peerplays.tech/supporting-and-reference-docs/peerplays-dex-development/peerplays-nex/functional-specifications/nex-fs05-market-page#9.-appendix-a-glossary) for more details on the order types.)
13. The order will appear in the order book and the open orders list. Any fulfilled trading (partial and/or full) will appear in the market and user trade history.
14. The user may cancel any remaining portion of the order from the open orders list.

## 6. Context

The market page contains in-depth functions to trade assets in Peerplays. This page should contain everything a user needs to understand the current state of the market to make informed decisions about their trades. The market page should provide all the tools a user would use to trade as with any industry standard exchange platform.

A review of modern exchange platforms has helped guide the requirements for the Peerplays NEX market page. See [section 10. Appendix B: Exchange Platforms Reference](https://devs.peerplays.tech/supporting-and-reference-docs/peerplays-dex-development/peerplays-nex/functional-specifications/nex-fs05-market-page#10.-appendix-b-exchange-platforms-reference) below.

## 7. Design Wire-frames

The original designs for the NEX app market page are available here:

[DEX Hifi - All tabs with Market](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

Here is an updated wire-frame for the design covered in this FS:

![](<../../../../.gitbook/assets/NEX-FS05 Market Page (Future).png>)

Figure 1. The Market Page design wire-frame.

Here is the same wire-frame including sample graphics to demonstrate a potential design implementation:

![](<../../../../.gitbook/assets/NEX-FS05 Market Page (Future Graphic).png>)

Figure 2. The Market Page design wire-frame with sample graphics.

## 8. Requirements

Requirements specific to the items listed in this FS are as follows.

### 8.1. Market Page Layout

The market page layout:

**NEX-FS05-1:** shall be available for authenticated and unauthenticated users within the application menu.

**NEX-FS05-2:** shall display the following components:

* market selector
* price and volume statistics
* order book
* market trade history
* buy / sell order controls
* time in force options
* price chart
* market depth chart
* user wallet functions (deposit and withdraw)
* user’s asset balances
* user’s open orders
* user’s order history (full and partial)

**NEX-FS05-3:** shall display widgets and components that have been configured to be active in the market page.

**NEX-FS05-4:** shall use graphic design elements which adhere to Peerplays branding guidelines.

**NEX-FS05-5:** shall use graphic design elements which remain consistent throughout the app.

**NEX-FS05-6:** shall allow user input in relevant form fields to perform the functions of the related component.

**NEX-FS05-7:** shall perform input field validation and inform the user of acceptable form inputs.

**NEX-FS05-8:** shall provide the user with help and/or hint text to explain available options and input fields.

**NEX-FS05-9:** shall provide unobtrusive help text to the user about each function. This may take any of the following forms:

* tooltip when hovering an element (button, title, etc.)
* pop-up when clicking an element (icon, link, etc.)
* expanding panels or menus with help text
* one time tutorial flow for new users
* similar techniques for revealing content

**NEX-FS05-10:** shall indicate to the user when content is loading and avoid showing incorrect, outdated, or blank content.

### 8.2. Order Book widget (market open orders)

The order book widget:

**NEX-FS05-11:** shall display a real-time aggregation of open buy and sell orders of the selected market pair.

**NEX-FS05-12:** shall display the following information about each open order:

* price (in quote asset)
* amount (of base asset) available per price
* total cost (in quote asset) per amount (of base asset)
* bar graph or other suitable indicator representing the volume of assets available per group of orders.

**NEX-FS05-13:** shall allow the user to select various threshold (precision) levels to group orders by price. (This could be done by number of decimal places)

**NEX-FS05-14:** shall display the market spread (gap in price between ask price and bid price) by displaying the following information:

* Icon representing if the last market taker trade was a buy (example: a green up arrow) or a sell (example: a red down arrow).
* price (quote asset) of the last market taker trade.
* value of the last market taker trade in the user’s preferred fiat (user settings).
* the value of the gap (quote asset) between the current ask price and current bid price.

**NEX-FS05-15:** the design of the market spread info shall indicate if the last market taker trade was a buy or sell with icons, arrows, or other design elements as well as color to help facilitate universal design implementation.

**NEX-FS05-16:** shall allow the user to select to view buy orders only, sell orders only, or both buy and sell orders.

**NEX-FS05-17:** shall allow the user to click a price level to apply that data to the buy / sell controls automatically.

**NEX-FS05-18:** the design shall indicate to the user buy orders (green) vs sell orders (red).

### 8.3. Trade History widget (market trade history)

The trade history widget:

**NEX-FS05-19:** shall display all recently executed market taker trades of the selected market pair arranged in a list chronologically from most recent.

* number of trades displayed may be based on time (past 12 hours)
* number of trades displayed may be based on number of trades (past 100 trades)

**NEX-FS05-20:** shall be updated in real-time.

**NEX-FS05-21:** shall include the trade quantity (base asset), price (quote asset), and timestamp.

**NEX-FS05-22:** the UI shall indicate if each market taker trade was a buy (green) or a sell (red) trade.

**NEX-FS05-23:** the design shall indicate a buy or sell with icons, arrows, or other design elements as well as color to help facilitate universal design implementation.

### 8.4. My Open Orders list (user open orders)

The my open order list:

**NEX-FS05-24:** shall include all of the user's currently open orders (regardless of selected market pair) with real-time data updates.

**NEX-FS05-25:** shall allow the user to toggle viewing open orders for the selected market pair vs all their open orders.

**NEX-FS05-26:** shall include the following data fields:

* the date and time each order was opened.
* the market pair
* if the order is a buy or sell ("side")
* the order type (limit, stop-limit, etc.)
* the order price (quote asset)
* the total order quantity (base asset)
* the filled order quantity value (base asset) and percentage of the order
* the total price (quote asset)
* the incurred fees
* the order status (open, partially filled, pending if stop-limit stop price hasn't been met)
* the expiry time (if applicable)
* may display a separate list of filled quantities of a partially filled order.

**NEX-FS05-27:** shall allow the user to sort the list by its data fields.

**NEX-FS05-28:** shall allow the user to filter the list by its data fields.

**NEX-FS05-29:** shall allow the user to cancel any open order, even if partially filled.

* user control for this may be a button or context menu item.

### 8.5. My Trade History list (user trade history)

The my trade history list:

**NEX-FS05-30:** shall include all of the user's completed trade activities (regardless of selected market pair) with real-time data updates.

**NEX-FS05-31:** shall allow the user to toggle viewing their trade history for the selected market pair vs all their trade history.

**NEX-FS05-32:** shall include the following data fields:

* the date and time each order was opened.
* the date and time each trade was completed (filled, partially filled, canceled).
* the market pair
* if the order was a buy or sell ("side")
* the order type (market, limit, stop-limit, etc.)
* the order price (quote asset)
* the filled order quantity (base asset)
* the total price (quote asset)
* the incurred fees
* the order status (partially filled, filled, canceled by user, canceled by system, expired)
* may display a separate list of filled quantities of a partially filled order.

**NEX-FS05-33:** shall allow the user to sort the list by its data fields.

**NEX-FS05-34:** shall allow the user to filter the list by its data fields.

### 8.6. Buy / Sell controls

The buy / sell controls:

**NEX-FS05-35:** shall allow the user to select between buy and sell order forms, via tabs or other means, to only display **one** order form to the user at any given time. In other words, users should not see the buy order form and sell order form on the page at the same time. (See the latest design wire-frame in section 6 above for an example)

**NEX-FS05-36:** shall allow the user to select among market, limit, and stop-limit order types.

**NEX-FS05-37:** shall display the user’s amount of assets available for trading (quote asset if buying, base asset if selling).

**NEX-FS05-38:** shall allow the user to input the order amount (quantity of base asset).

**NEX-FS05-39:** shall provide convenience controls to the user which allows them to easily adjust the order amount automatically to any percentage of their assets available to trade. For example, supply a slider user input control which allows input from 0% to 100% of their available assets. This slider could even have nodes that allow easy selection of 0%, 25%, 50%, 75%, and 100%, but still allows any whole number percentage in between.

**NEX-FS05-40:** shall, if the order type is a limit order, allow the user to input the limit price (quote asset).

**NEX-FS05-41:** shall, in an “advanced” menu and if the order type is a limit or stop-limit order, allow the user to select a time in force policy option (only one):

* good-til-canceled (default)
* good-til-time
* fill-or-kill
* maker-or-cancel
* immediate-or-cancel

**NEX-FS05-42:** shall, in an “advanced” menu and if the order type is a limit or stop-limit order, allow the user to select an order execution option (only one):

* post only
* allow taker (default)

**NEX-FS05-43:** shall, if the order type is a stop-limit order, allow the user to input the limit price (quote asset) and stop price (quote asset).

**NEX-FS05-44:** shall, if the order type is a limit or stop-limit order, provide convenience controls to the user which allows them to automatically fill the price field with common strategic price points. For example, supply buttons which fill the price of a **buy** order at:

* MID (at the current price)
* BID (at the bid price)
* 1% down (1% lower than the current price)
* 5% down (5% lower than the current price)
* 10% down (10% lower than the current price)

Another example, supply buttons which fill the price of a **sell** order at:

* MID (at the current price)
* ASK (at the ask price)
* 1% up (1% higher than the current price)
* 5% up (5% higher than the current price)
* 10% up (10% higher than the current price)

**NEX-FS05-45:** shall display to the user the fees of the order, given the user’s inputs.

**NEX-FS05-46:** shall, if the order is a market order, display to the user the estimated slippage of the order, given the user’s inputs and current market conditions.

**NEX-FS05-47:** shall, if the order is a market order, display to the user the estimated average price (quote asset) of the order, given the user’s inputs and current market conditions.

**NEX-FS05-48:** shall display to the user the total (quote asset) of the order, given the user’s inputs.

**NEX-FS05-49:** shall allow the user to create an order if the following criteria are met:

* the user is logged in.
* the order form is filled in with (successfully) validated inputs.
* the user has an appropriate amount of assets to satisfy the needs of the order.

**NEX-FS05-50:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS05-51:** shall provide user controls (buttons) to initiate buy and sell orders.

**NEX-FS05-52:** shall, once initiated by the user, summarize the order and ask the user for their confirmation to proceed.

**NEX-FS05-53:** shall, once initiated or confirmed by the user, indicate order is in process. (loading spinner)

**NEX-FS05-54:** shall, once confirmed by the user, submit the order to the Peerplays network.

**NEX-FS05-55:** shall indicate to the user if the order was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS05-56:** shall, if the order initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS05-57:** shall, upon successful order initiation, add the order to the order book, update the user’s asset balances, add the order to the user’s open orders (for limit orders), etc.

### 8.7. Price Chart

The price chart:

**NEX-FS05-58:** shall display a real-time chart of price data (quote asset) for the selected market.

**NEX-FS05-59:** shall include standard price information for the currently selected chart timeframe:

* open price
* high price
* low price
* close price
* current price (the executed price of the last market taker trade)
* change from last bar on chart (value and percentage)
* ask price
* bid price
* volume

**NEX-FS05-60:** shall allow the selection of chart timeframe with standard timeframes:

* 1 minute (M1)
* 5 minutes (M5)
* 15 minutes (M15)
* 30 minutes (M30)
* 1 hour (H1)
* 2 hours (H2)
* 4 hours (H4)
* 6 hours (H6)
* 1 day (D1)
* 1 week (W1)
* 1 month (M1)
* may allow the selection of other timeframes, preset or custom.

**NEX-FS05-61:** shall allow the selection of chart historical range with standard ranges:

* 4 hours (H4)
* 1 day (D1)
* 1 week (W1)
* 1 month (M1)
* 3 months (M3)
* 6 months (M6)
* year to date (YTD)
* 1 year (Y1)
* 5 years (Y5)
* all (ALL)
* go to date (opens a modal to pick the date)
* custom range (opens a modal to pick the range)
* may allow the selection of other ranges, preset or custom.

**NEX-FS05-62:** shall display time along the x-axis and price along the y-axis.

**NEX-FS05-63:** shall allow for zooming the chart in and out.

**NEX-FS05-64:** shall allow for adjusting the time scale and price scale.

**NEX-FS05-65:** shall allow for dragging the chart to view historical prices.

**NEX-FS05-66:** shall allow for viewing individual bar price data. For example, hovering the mouse on a historical price bar for its open, high, low, close, and volume info.

**NEX-FS05-67:** shall allow the selection of chart type:

* candlesticks
* line chart
* bar chart
* may allow the selection of other chart types.

**NEX-FS05-68:** shall allow for adding and removing technical indicators:

* moving averages
* may allow the selection of other technical indicators.

### 8.8. Price / Volume statistics

The price / volume statistics:

**NEX-FS05-69:** shall display the last price (quote asset) for the selected market pair.

**NEX-FS05-70:** last price display shall indicate if the last market taker trade was a buy (green) or sell (red).

**NEX-FS05-71:** last price display shall indicate if the last market taker trade was a buy or sell with icons, arrows, or other design elements as well as color to help facilitate universal design implementation.

**NEX-FS05-72:** shall display the last price in the user’s preferred currency (user settings).

**NEX-FS05-73:** shall display the Bid and Ask prices (quote asset).

**NEX-FS05-74:** shall display the 24-hour price change in terms of raw value and in percentage (quote asset).

**NEX-FS05-75:** the 24-hour price change display, for both the raw value and percentage displays, shall indicate net positive (green), negative (red), or no change (neutral) using colors as well as signs (+ or -).

**NEX-FS05-76:** shall display the 24-hour price range (high and low, in quote asset).

**NEX-FS05-77:** shall display the 24-hour trade volume (quote asset).

**NEX-FS05-78:** shall update the price and volume statistics in real-time based on current market conditions.

**NEX-FS05-79:** each of the above displays (last price, ask, bid, 24h change, 24h high, 24h low, and 24h volume) shall be labeled.

### 8.9. Market Selector

The market selector:

**NEX-FS05-80:** shall display the currently selected market (trading pair).

**NEX-FS05-81:** shall allow the user to select any available market.

**NEX-FS05-82:** shall allow the user to flip the base and quote assets of the trading pair. For example, users need to be able to change the selected market from BTC-ETH to ETH-BTC.

**NEX-FS05-83:** shall, when a new market is selected by the user, update the page to reflect the selected market. (order book, history, asset balances, prices, etc.)

### 8.10. Balances Display

The balances display:

**NEX-FS05-84:** shall display the user's asset balances available for trading relating to the currently selected market.

**NEX-FS05-85:** shall update the available balances in the display in real-time when orders are created, filled, partially filled, canceled, or otherwise changed.

### 8.11. Market Depth Chart

The depth chart:

**NEX-FS05-86:** shall update its values in real-time.

**NEX-FS05-87:** shall display two area charts: one for all buy orders, and one for all sell orders.

**NEX-FS05-88:** shall, for buy orders, represent the amount of base asset (Y-axis) on order at each amount of quote asset (X-axis) or lower.

**NEX-FS05-89:** shall, for sell orders, represent the amount of base asset (Y-axis) on order at each amount of quote asset (X-axis) or higher.

**NEX-FS05-90:** shall display the current market (mid market) price.

**NEX-FS05-91:** shall allow the user to zoom the chart in and out.

**NEX-FS05-92:** shall allow the user to hover on the chart to view more specific data:

* amount on order
* specific price and change from the mid market (percentage)
* total price

**NEX-FS05-93:** shall indicate buy orders (green) and sell orders (red).

### 8.12. User Wallet Functions

**NEX-FS05-94:** shall allow the user to initiate an asset deposit (for the quote or base asset).

**NEX-FS05-95:** shall allow the user to initiate an asset withdraw (for the quote or base asset).

### 8.13. Balances Display

**NEX-FS05-96:** shall display the user’s amount of quote asset available to trade (not on orders, etc.)

**NEX-FS05-97:** shall display the user’s amount of base asset available to trade (not on orders, etc.)

**NEX-FS05-98:** shall display accurate information in real-time (updated with order activity.)

## 9. Appendix A: Glossary

| Term                | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RS                  | Requirements Specification                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| FS                  | Functional Specification                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| UI                  | User Interface                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Market Order        | Orders that are meant to execute as quickly as possible at the current market price. If you’re buying an asset, a market order will execute at whatever price the seller is asking. If you’re selling, a market order will execute at whatever the buyer is bidding. The biggest drawback of the market order is that you can’t specify the price of the trade. Market orders cannot be canceled because they are filled immediately. Market orders may be partially filled at several prices. |
| Limit Order         | An order that sets the maximum or minimum price at which you are willing to buy or sell. The biggest advantage of the limit order is that you get to name your price, and if the asset reaches that price, the order will be filled.                                                                                                                                                                                                                                                           |
| Good Til Canceled   | A type of Limit order (the default limit order.) A Good Til Canceled Limit order will stay on the Order Book until it's 100% filled or canceled. Even if the order has partial fills, it will stay on the books.                                                                                                                                                                                                                                                                               |
| Good Til Time       | A type of Limit order. A Good Til Time Limit order will stay on the Order Book until it's 100% filled, canceled, or its specified duration has expired. Even if the order has partial fills, it will stay on the books until the expiration.                                                                                                                                                                                                                                                   |
| Immediate-or-Cancel | A type of Limit order. This order will be placed and if it is not immediately filled, it will automatically be canceled and removed from the order book. Note that it may be partially filled and then the unfilled portion is canceled.                                                                                                                                                                                                                                                       |
| Fill-or-Kill        | A type of Limit order. This order will only complete if the entire amount can be matched. Partial matches are not filled with this order type and will not execute.                                                                                                                                                                                                                                                                                                                            |
| Maker-or-Cancel     | A type of Limit order. It's the opposite of a Fill-or-Kill. This order will only be placed in the order book if the entire amount is not immediately filled. If any portion of this order can be immediately filled, it will be canceled.                                                                                                                                                                                                                                                      |
| Stop-Limit Order    | Stop-Limit orders allow you to buy or sell when the price reaches a specified value, known as the stop price. This order type helps traders protect profits, limit losses, and initiate new positions. A Stop-Limit order will automatically post a limit order at the limit price when the stop price is triggered. Note that the stop order will be triggered instantly if the stop price specified was already met.                                                                         |

## 10. Appendix B: Exchange Platforms Reference

The following platforms have been reviewed to help guide the requirements of the Peerplays NEX market page offerings.

* [p2pb2b.io](https://p2pb2b.io/)
* [Gemini](https://www.gemini.com/)
* [Coinbase Pro](https://pro.coinbase.com/)
* [Coinbase Advanced](https://coinbase.com)
* [Binance.us](https://www.binance.us)
* [FTX Exchange (US)](https://ftx.us)
