---
description: >-
  The Peerplays NEX application functional requirements specification for the
  wallet functions.
---

# NEX-FS07 Wallet Functions

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the wallet functions from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`process`

`page fragment`

## 3. Scope

This FS will describe the requirements and basic design for the app’s wallet design and functions.

### 3.1. Components

Specific components and features covered in this FS include:

* wallet page layout
* user asset list
* send functions
* receive functions

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                                |
| ---------- | -------------------------------------- |
| NEX-FS07-# | NEX App Requirement - Wallet Functions |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

## 5. Process Overviews

The processes which will be described here are as follows.

* sending assets
* receiving assets

### 5.1. Sending Assets

1. The user navigates to the wallet send function.
2. The user reviews their assets available to send.
3. The user selects an asset in the asset selection field.
4. The send form updates as necessary per the selected asset.
5. The user enters an amount of available asset to send in the amount field.
6. The send form updates as necessary per the user inputs.
7. The user enters the recipient account name into the to field.
8. The user selects which blockchain to send the funds to, if withdrawing to a sidechain.
9. The user may enter a memo if sending assets within the Peerplays network.
10. The user reviews the send transaction information, such as fees, total costs, and estimated confirmation time.
11. The user initiates the transaction by clicking the “send” button.
12. The app displays a confirmation dialog to the user.
13. The user reviews and accepts the confirmation.
14. The app signs and sends the transaction to the blockchain.
15. Upon success, the app displays a success message to the user and updates the user’s available balances.

### 5.2. Receiving Assets

1. The user navigates to the wallet receive function.
2. The user selects an asset in the asset selection field.
3. The receive form updates as necessary per the selected asset.
4. The user reviews the instructions for receiving the asset they selected.
5. If the selected asset is BTC, the app displays their BTC deposit address and allows the user to cope the address to their clipboard.
6. If the selected asset is BTC and a BTC deposit address is not yet associated with the user’s Peerplays account, the app displays a “Generate Bitcoin Address” function.
7. The user clicks the “Generate Bitcoin Address” button.
8. The app generates a new BTC deposit and BTC withdraw address, displays the BTC deposit address to the user, and displays a download link for the deposit and withdraw address private keys.
9. The user downloads their private keys.

## 6. Context

The wallet is at the heart of the app. It’s the way assets can come and go from the market. Using the wallet, users can work seamlessly with sidechain assets like BTC and HIVE. These assets can then be traded in the open exchange, sent around the network, and even sent back to the sidechains.

## 7. Design Wire-frames

The original designs for the NEX app wallet functions are available here:

[DEX Hifi - All tabs with Market](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

Here are updated wire-frames for the design covered in this FS:

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Assets.drawio.png>)

Figure 1. The user’s assets list in the wallet.

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Send (Blank).drawio.png>)

Figure 2. The send function (blank form) in the wallet.

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Send (PPY).drawio.png>)

Figure 3. The send function (PPY example) in the wallet.

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Send (BTC).drawio.png>)

Figure 4. The send function (BTC example) in the wallet.

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Send (HIVE).drawio.png>)

Figure 5. The send function (HIVE example) in the wallet.

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Receive (Blank).drawio.png>)

Figure 6. The receive function (blank form) in the wallet.

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Receive (PPY).drawio.png>)

Figure 7. The receive function (PPY example) in the wallet.

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Receive (BTC).drawio.png>)

Figure 8. The receive function (BTC example) in the wallet.

![](<../../../../.gitbook/assets/NEX-FS06 Wallet Functions Receive (HIVE).drawio.png>)

Figure 9. The receive function (HIVE example) in the wallet.

## 8. Requirements

Requirements specific to the items listed in this FS are as follows.

### 8.1. Wallet Page Layout

The wallet, in the context of the page layout:

**NEX-FS07-1:** shall be available for authenticated users within the application menu.

**NEX-FS07-2:** shall provide the user with navigation to the following pages:

* the app dashboard
* the market page
* the user profile page
* the user wallet
* the app settings
* the blockchain blocks page (if advanced mode is active)
* the voting (GPOS) page (if advanced mode is active)

**NEX-FS07-3:** shall provide menus for displaying the following components:

* user’s assets list
* wallet send function
* wallet receive function

**NEX-FS07-4:** shall use graphic design elements which adhere to Peerplays branding guidelines.

**NEX-FS07-5:** shall use graphic design elements which remain consistent throughout the app.

**NEX-FS07-6:** shall allow user input in relevant form fields to perform the functions of the related component.

**NEX-FS07-7:** shall perform input field validation and inform the user of acceptable form inputs.

**NEX-FS07-8:** shall provide the user with help and/or hint text to explain available options and input fields.

**NEX-FS07-9:** shall provide unobtrusive help text to the user about each function. This may take any of the following forms:

* tooltip when hovering an element (button, title, etc.)
* pop-up when clicking an element (icon, link, etc.)
* expanding panels or menus with help text
* one time tutorial flow for new users
* similar techniques for revealing content

**NEX-FS07-10:** shall indicate to the user when content is loading and avoid showing incorrect, outdated, or blank content.

**NEX-FS07-11:** displayed lists shall provide pagination controls for large record sets requiring multiple pages.

**NEX-FS07-12:** displayed lists shall display information about their records in a tabular format.

**NEX-FS07-13:** displayed lists shall display a header row with column names.

**NEX-FS07-14:** displayed lists shall provide controls in the header to sort columns based on their values, if appropriate.

**NEX-FS07-15:** displayed lists shall display columns of a width appropriate for their values.

**NEX-FS07-16:** displayed lists shall highlight row which the user is hovering on.

**NEX-FS07-17:** displayed lists shall highlight row which the user has clicked on (focused).

### 8.2. Assets List

The wallet, in the context of the assets list:

**NEX-FS07-18:** shall display a list of the user’s available assets. May display two separate lists: one for coins and tokens, the other for NFTs.

**NEX-FS07-19:** shall include the following fields for each available asset:

* asset symbol
* asset name
* amount available
* amount in orders
* total amount owned (available + in orders)
* total value
* actions
  * send
  * receive

**NEX-FS07-20:** shall display the total value in the user’s preferred currency (app settings).

**NEX-FS07-21:** shall, upon activating the “send” action, navigate to the wallet send function and populate the asset selection with the corresponding asset.

**NEX-FS07-22:** shall, upon activating the “receive” action, navigate to the wallet receive function and populate the asset selection with the corresponding asset.

**NEX-FS07-23:** shall display the total value of **all** assets (excluding NFTs) in the user’s wallet in the user’s preferred currency (app settings).

**NEX-FS07-24:** shall allow the user to sort the list by its data fields.

**NEX-FS07-25:** shall allow the user to filter the list by its data fields.

**NEX-FS07-26:** shall allow the user to open and close the list (expandable menu).

**NEX-FS07-27:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 8.3. Send Function

The wallet, in the context of the send function:

**NEX-FS07-28:** shall provide the following user input controls:

* asset to send (drop down)
* amount to send (numeric field)
* convenience slider control for sending percentages of available balance (See NEX-FS07-29)
* recipient account (or recipient address for BTC)
* blockchain (drop down)
* memo (text field, optional)
* “Send” button to initiate the send function
* “clear form” function

**NEX-FS07-29:** shall provide convenience controls to the user which allows them to easily adjust the send amount automatically to any percentage of their asset available to send. For example, supply a slider user input control which allows input from 0% to 100% of their available asset balance. This slider could even have nodes that allow easy selection of 0%, 25%, 50%, 75%, and 100%, but still allows any whole number percentage in between.

**NEX-FS07-30:** shall display the following information relevant to the send transaction:

* amount of selected asset available to send
* estimated fees
* transaction total
* estimated confirmation time (length of time)

**NEX-FS07-31:** shall allow the selection of any asset for which the user has an available balance.

**NEX-FS07-32:** shall, upon selecting an asset, update the form controls in the following ways:

* (PPY selected) blockchain selection set to Peerplays and disabled.
* (BTC selected and Bitcoin blockchain selected) memo is disabled.
* (HIVE selected and Hive blockchain selected) memo is disabled.

**NEX-FS07-33:** shall, upon selecting the “clear form” function, clear the form back to its initial state.

**NEX-FS07-34:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS07-35:** shall, once initiated by the user, summarize the send and ask the user for their confirmation to proceed.

**NEX-FS07-36:** shall, once initiated or confirmed by the user, indicate send is in process. (loading spinner)

**NEX-FS07-37:** shall, once confirmed by the user, submit the transaction to the Peerplays network.

**NEX-FS07-38:** shall indicate to the user if the send was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS07-39:** shall, if the send initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS07-40:** shall, upon successful send initiation, update the user’s asset balances.

**NEX-FS07-41:** shall display a list of the user’s available assets (excluding NFTs).

**NEX-FS07-42:** shall include the following fields for each available asset:

* asset symbol
* asset name
* amount available (to send)
* amount in orders
* total amount owned (available + in orders)
* total value
* actions
  * select this asset

**NEX-FS07-43:** shall display the total value in the user’s preferred currency (app settings).

**NEX-FS07-44:** shall, upon activating the “select this asset” action, populate the asset selection with the corresponding asset.

**NEX-FS07-45:** shall allow the user to sort the list by its data fields.

**NEX-FS07-46:** shall allow the user to filter the list by its data fields.

**NEX-FS07-47:** shall allow the user to open and close the list (expandable menu).

### 8.4. Receive Function

The wallet, in the context of the receive function:

**NEX-FS07-48:** shall provide the following user input controls:

* asset to receive (drop down)

**NEX-FS07-49:** shall allow the selection of any asset which can be received on the Peerplays network.

**NEX-FS07-50:** shall, upon selecting an asset, update the form controls in the following ways:

* (PPY selected) display help text for receiving PPY to a Peerplays account.
* (BTC selected) display help text for receiving BTC to a Peerplays account.
* (HIVE selected) display help text for receiving HIVE to a Peerplays account.

**NEX-FS07-51:** shall, if BTC is the selected asset, check if the user has a BTC sidechain address which is associated with their account. If not, display a “Generate Bitcoin Address” function. If so, display the BTC deposit address with a function to copy the address to the user’s clipboard.

**NEX-FS07-52:** shall, when generating a new BTC address, generate a new BTC deposit and withdraw address and associate them with the user’s Peerplays account.

**NEX-FS07-53:** shall, when generating a new BTC address, display the newly generated BTC deposit address with a function to copy the address to the user’s clipboard.

**NEX-FS07-54:** shall, when generating a new BTC address, display a link to download the BTC deposit and withdraw address private keys and a warning for the user to do so.

**NEX-FS07-55:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS07-56:** shall display a list of the user’s available assets (excluding NFTs).

**NEX-FS07-57:** shall include the following fields for each available asset:

* asset symbol
* asset name
* amount available (to send)
* amount in orders
* total amount owned (available + in orders)
* total value
* actions
  * select this asset

**NEX-FS07-58:** shall display the total value in the user’s preferred currency (app settings).

**NEX-FS07-59:** shall, upon activating the “select this asset” action, populate the asset selection with the corresponding asset.

**NEX-FS07-60:** shall allow the user to sort the list by its data fields.

**NEX-FS07-61:** shall allow the user to filter the list by its data fields.

**NEX-FS07-62:** shall allow the user to open and close the list (expandable menu).

## 9. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
