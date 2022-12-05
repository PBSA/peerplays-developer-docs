---
description: >-
  The Peerplays NEX application functional requirements specification for the
  dashboard page.
---

# NEX-FS01 Dashboard Page

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the dashboard page from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`Page`

`process`

## 3. Scope

This FS will describe the requirements and basic design for the app’s dashboard page. The non-native assets currently available within Peerplays (BTC, HIVE, and HBD) will be covered in detail. Note that HIVE and HBD operate identically in the app and are interchangeable for the purposes of this FS.

### 3.1. Components

Specific components and features covered in this FS include:

* dashboard page layout
* dashboard asset deposit functions
* dashboard asset withdraw functions
* dashboard asset swap functions
* dashboard market listings function

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                              |
| ---------- | ------------------------------------ |
| NEX-FS01-# | NEX App Requirement - Dashboard Page |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

`Peerplays assets` in the context of this FS are any on-chain asset. This includes native Peerplays assets like the PPY coin, Peerplays NFTs, and CATs. This also includes assets that have originated off-chain that have been transferred onto the Peerplays chain through the services of Peerplays SONs. These external (sidechain) assets include Peerplays versions of BTC, HIVE, or ETH and even Peerplays versions of NFTs living on other chains. The external assets are backed by their counterparts, locked in a Peerplays controlled account on their native chains.

## 5. Process Overviews

The processes which will be described here are as follows.

* Asset Deposits
  * BTC
  * HIVE (or HBD)
* Asset Withdrawals
  * BTC
  * HIVE (or HBD)
* Asset Swaps
* Viewing and Selecting Trading Pairs

The following process overviews assume the example user is authenticated in the app and has an appropriate balance in their wallet for all assets involved.

### 5.1. Asset Deposits

#### 5.1.1. Depositing BTC

1. A visitor (non-logged in user) navigates to the dashboard page’s deposit tab.
2. The app displays a screen which has multiple available coins from sidechains supported by Peerplays (BTC, HIVE and HBD) shown in a drop down. Selecting an asset populates rest of UI depending on the asset selected.
3. The visitor invokes the "Login & Generate Bitcoin address" button. Since they are not logged in, this will prompt the visitor to input their Peerplays username and MASTER PASSWORD. (See Adobe XD pages: [1](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/) & [2](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/0baa5f25-065c-4827-bb0a-fb8b8d160676/) )
4. If the visitor already had a Bitcoin deposit address generated, it is shown with an option to copy the address. The page also displays help text or a link to the help text describing the steps to deposit BTC.
5. If the visitor does not have a Peerplays account, they can navigate to the create account page via a link. ([Adobe XD page 16](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/3adc0565-1624-476c-8f6e-41f938fb6a03/))
6. At this point the visitor is logged in and the client side Bitcoin library generates a Bitcoin deposit address & private key. The page displays a link to download the private key as a text file named `Bitcoin_addresses_<PeerplaysUserName>.txt` which contains the private key of the newly generated Bitcoin addresses (deposit and withdrawal). ([Adobe XD screen 3](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/09e75b43-3f15-4ca3-b0c6-9d4254b275a3/)) . The private key will be available only once and is not stored by the NEX software or the blockchain and thus appropriate warning messages are shown to the user instructing them to safely save the secret.
7. The user is additionally given an option to copy the Bitcoin deposit address. ([Adobe XD screen 3](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/09e75b43-3f15-4ca3-b0c6-9d4254b275a3/))
8. Once a user has generated a Bitcoin deposit address, the same address will be shown in the subsequent logins and the options to re-generate will not be presented.
9. External to the app, the user sends an amount of BTC to the deposit Bitcoin address.
10. The user returns to the app and is eventually notified in the app when their account receives the BTC.

#### 5.1.2. Depositing HIVE (or HBD)

1. The user navigates to the dashboard page’s deposit tab.
2. The app displays the asset deposit functions.
3. The user selects the Hive (HIVE) asset from a list of assets available for deposit.
4. The page loads instructions for initiating a HIVE deposit to the user’s app account.
5. External to the app, the user sends an amount of HIVE to the Hive son-account along with a memo including their account name, as per the instructions in the app.
6. The user returns to the app and is eventually notified in the app when their account receives the HIVE.

### 5.2. Asset Withdrawals

#### 5.2.1. Withdrawing BTC

1. The user navigates to the dashboard page’s withdraw tab.
2. The app displays the asset withdraw functions.
3. The user selects the Bitcoin (BTC) asset from a list of assets available for withdraw.
   1. If the user has not already created a Bitcoin deposit address, the app displays a message to the user explaining they must first create a deposit address. A link to the deposit tab is displayed to the user. (See section 5.1.1. above)
   2. If the user has already created a Bitcoin deposit address, the app displays the BTC asset withdraw form.
4. The page loads instructions for initiating a BTC withdraw from the user’s app account. The page also contains a Bitcoin withdraw address field. This field contains a generated withdraw address based on the deposit address generated previously.
   1. If the user wishes, they can generate another Bitcoin withdraw address.
5. The user reviews their wallet BTC balance and enters an appropriate amount of BTC they wish to withdraw.
6. The app looks up the public key for the Bitcoin withdraw account.
7. The user clicks a button to submit the withdrawal.
8. The app displays a modal to the user asking them to confirm their withdrawal.
9. The user reviews the withdrawal information and clicks to confirm the withdrawal.
10. The app processes and submits the withdrawal to the SON network.
11. The app indicates to the user that the withdrawal has been successfully initiated.
12. The content displayed in the app is updated to show new available balances, etc.

#### 5.2.2. Withdrawing HIVE (or HBD)

1. The user navigates to the dashboard page’s withdraw tab.
2. The app displays the asset withdraw functions.
3. the user selects the Hive (HIVE) asset from a list of assets available for withdraw.
4. The page loads instructions for initiating a HIVE withdraw from the user’s app account. The page also contains a Hive blockchain account name field.
5. The user reviews their wallet HIVE balance and enters an appropriate amount of HIVE they wish to withdraw.
6. The user enters a valid Hive blockchain account name with which they wish to receive their withdrawal.
7. The app looks up the Hive blockchain account name to ensure its validity.
8. The user clicks a button to submit the withdrawal.
9. The app displays a modal to the user asking them to confirm their withdrawal.
10. The user reviews the withdrawal information and clicks to confirm the withdrawal.
11. The app processes and submits the withdrawal to the SON network.
12. The app indicates to the user that the withdrawal has been successfully initiated.
13. The content displayed in the app is updated to show new available balances, etc.

### 5.3. Asset Swaps

1. The user navigates to the dashboard page’s swap tab.
2. The app displays the asset swap functions.
3. The user selects an asset they wish to spend (BTC for example) to swap for another asset.
4. The user selects an appropriate amount of BTC to swap.
5. The user selects an asset they wish to receive (PPY for example) for their BTC.
6. The app calculates and displays the amount of PPY they’re likely to receive for this swap.
7. The app displays the exchange rate, fee, and transaction type for this swap.
8. The user reviews the information and clicks a button to submit the swap.
9. The app displays a modal to the user asking them to confirm their swap.
10. The user reviews the swap information and clicks to confirm the swap.
11. The app processes and submits the market order to the Peerplays network.
12. The app indicates to the user that the swap has been successfully initiated.
13. The content displayed in the app is updated to show new available balances, etc.
14. When the market order resolves, the user is notified in the app of the outcome of their swap.

### 5.4. Viewing and Selecting Trading Pairs

1. The user navigates to the dashboard page’s market tab.
2. The app displays all available trading pairs supported by the app market.
3. The user reviews the trading pair information and clicks a trading pair they wish to visit in the app market.
4. The app loads the market page with the selected trading pair loaded.
5. The user continues in the market page.

## 6. Context

The dashboard page holds many of the most used functions of the app to facilitate ease of use. These functions are simplified to cover most user’s needs while removing as much distraction and complexity as possible. The dashboard page can be used as a homepage or a location within the top level navigation so users can quickly get their tasks done.

## 7. Design Wire-frames

Designs for the NEX app dashboard page are available here:

[DEX Hifi - All tabs with Market](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

## 8. Requirements

Requirements specific to the items listed in this FS are as follows.

### 8.1. Dashboard page layout

The dashboard:

**NEX-FS01-1:** shall provide access to the following functions for authenticated users:

* asset deposits
* asset withdrawals
* asset swaps
* market trading pair listings

**NEX-FS01-2:** shall provide access to the following functions for unauthenticated users:

* log in and deposit asset
* log in and withdraw asset
* log in and swap assets
* market trading pair listings
* link to create an account

**NEX-FS01-3:** shall provide unobtrusive help text to the user about each function. This may take any of the following forms:

* tooltip when hovering an element (button, title, etc.)
* pop-up when clicking an element (icon, link, etc.)
* expanding panels or menus with help text
* one time tutorial flow for new users
* similar techniques for revealing content

**NEX-FS01-4:** shall indicate to the user when content is loading and avoid showing incorrect, outdated, or blank content.

### 8.2. Dashboard asset deposit functions

The dashboard, in the context of asset deposits:

**NEX-FS01-5:** shall allow the selection of any available Peerplays asset for deposit.

**NEX-FS01-6:** shall display asset icons, asset names (like “Bitcoin”), and asset symbols (like “BTC”) for each asset in the asset selection user control.

**NEX-FS01-7:** shall display deposit instructions relevant to the selected asset.

**NEX-FS01-8:** shall, when necessary, indicate to the user that the SONs network is not available for sidechain transactions.

**NEX-FS01-9:** shall, where appropriate, display information required for depositing the selected asset. This may include, but is not limited to, the following:

* account names
* addresses
* keys
* memos
* files
* URIs
* QR codes

**NEX-FS01-10:** shall, where appropriate, facilitate copying or otherwise obtaining the information required for depositing the selected asset.

#### 8.2.1. BTC Deposits

While the BTC asset is selected in the asset deposit function:

**NEX-FS01-11:** shall display the Bitcoin icon in the deposit instructions.

**NEX-FS01-12:** shall display the users Bitcoin deposit address.

**NEX-FS01-13:** shall allow the user to copy their Bitcoin deposit address.

#### 8.2.2. HIVE (HBD) Deposits

While the HIVE (or HBD) asset is selected in the asset deposit function:

**NEX-FS01-14:** shall display the Hive icon in the deposit instructions.

**NEX-FS01-15:** shall display deposit instructions with the following text, or similar:

“To deposit (HIVE or HBD here) to (user account name here) please send your funds to son-account on the Hive blockchain with the memo (user account name here).”

### 8.3. Dashboard asset withdrawal functions

The dashboard, in the context of asset withdrawals:

**NEX-FS01-16:** shall allow the selection of any available Peerplays asset for withdraw.

**NEX-FS01-17:** shall display asset icons, asset names (like “Bitcoin”), and asset symbols (like “BTC”) for each asset in the asset selection user control.

**NEX-FS01-18:** shall display the user’s current balance of the selected asset available for withdraw.

**NEX-FS01-19:** shall display the fees (amount and asset) required to complete a withdraw.

**NEX-FS01-20:** shall allow the selection of an amount of asset to withdraw.

**NEX-FS01-21:** shall display withdraw instructions relevant to the selected asset.

**NEX-FS01-22:** shall only allow positive numbers with decimal places based on the selected asset within the withdrawal amount field. For example, Bitcoin supports 8 decimal places while Hive only supports 3 decimal places.

**NEX-FS01-23:** shall perform field input validation which validates the following rules:

* Input is not blank
* Input is a number
* Input amount is greater than 0
* Input amount is greater than the minimum withdrawal
* Balance in wallet covers withdrawal amount plus fees

**NEX-FS01-24:** shall clearly indicate to the user when field validation fails and why.

**NEX-FS01-25:** shall, where appropriate, provide user controls required to complete a withdraw of the selected asset. These user controls should perform field input validation as necessary. For example, required fields should not be blank, account names should be entered in their proper format, inputs should match the field types, etc.

**NEX-FS01-26:** shall, when necessary, indicate to the user that the SONs network is not available for sidechain transactions.

**NEX-FS01-27:** shall provide a user control (button) to initiate the withdraw.

**NEX-FS01-28:** shall, once initiated by the user, summarize the withdraw and ask the user for their confirmation to proceed.

**NEX-FS01-29:** shall, once initiated or confirmed by the user, indicate the withdraw is in process. (loading spinner)

**NEX-FS01-30:** shall, once confirmed by the user, submit the withdraw to the Peerplays SONs network.

**NEX-FS01-31:** shall indicate to the user if the withdraw was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS01-32:** shall, if the withdraw process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

#### 8.3.1. BTC Withdrawals

While the BTC asset is selected in the asset withdraw function:

**NEX-FS01-33:** shall display the user’s withdraw Bitcoin address, if available.

**NEX-FS01-34:** shall allow the user to generate a new withdraw Bitcoin address.

**NEX-FS01-35:** shall **not** display the user’s withdraw Bitcoin public key. The app may still make use of the public key to facilitate the process but does not need to be shown to the user.

**NEX-FS01-36:** shall lookup or otherwise generate the Bitcoin public key for any newly generated withdraw Bitcoin address entered by the user. Once again, this is not displayed to the user but used to facilitate the withdraw process.

**NEX-FS01-37:** shall allow the user to download their public and private keys for a newly generated Bitcoin address as a text file named `Bitcoin_addresses_<PeerplaysUserName>.txt`.

**NEX-FS01-38:** shall display appropriate warning messages indicating to the user that their newly generated Bitcoin address private key will not be saved by the system and instructs them to safely save the secret.

#### 8.3.2. HIVE (HBD) Withdrawals

While the HIVE (or HBD) asset is selected in the asset withdraw function:

**NEX-FS01-39:** shall provide a user control to allow a user to input their Hive blockchain account name.

**NEX-FS01-40:** shall perform field validation to ensure proper input format in the Hive blockchain account name field.

### 8.4. Dashboard asset swap functions

The dashboard, in the context of asset swaps:

**NEX-FS01-41:** shall display two asset selection fields: one to swap from and one to swap to.

**NEX-FS01-42:** shall allow the selection of any available asset and any other available asset.

**NEX-FS01-43:** shall display asset icons, asset names (like “Bitcoin”), and asset symbols (like “BTC”) for each asset in the asset selection user controls.

**NEX-FS01-44:** shall display the user’s current balance of the selected asset available for swap.

**NEX-FS01-45:** shall display the fees (amount and asset) required to complete a swap.

**NEX-FS01-46:** shall allow the selection of an amount of asset to swap.

**NEX-FS01-47:** shall, after the user updates the amount of asset to swap, query open orders in the market order book to:

* validate that there is liquidity for the trading pair
* get the current exchange rate for the trading pair

**NEX-FS01-48:** shall indicate to the user if not enough liquidity exists to fulfill the swap and to try again later.

**NEX-FS01-49:** shall, while the user updates the amount of asset to swap, change the other amount field to match the current exchange rate. In other words a user can chose either how much to spend of an asset they have, or how much they would like to receive of the asset they are swapping for. Either way, swaps are meant to occur at the current market exchange rate like a fill-or-kill market order.

**NEX-FS01-50:** shall display the value of the amounts in the asset selection fields in the user’s preferred currency (an app setting).

**NEX-FS01-51:** shall only allow positive numbers with decimal places based on the selected asset within the swap amount field. For example, Bitcoin supports 8 decimal places while Hive only supports 3 decimal places.

**NEX-FS01-52:** shall perform field input validation which validates the following rules:

* Input is not blank
* Input is a number
* Input amount is greater than 0
* Input amount is greater than the minimum swap
* Balance in wallet covers swap amount plus fees

**NEX-FS01-53:** shall clearly indicate to the user when field validation fails and why.

**NEX-FS01-54:** shall, when necessary, indicate to the user that the Peerplays network is not available for swap transactions.

**NEX-FS01-55:** shall provide a user control (button) to initiate the swap.

**NEX-FS01-56:** shall, once initiated by the user, summarize the swap and ask the user for their confirmation to proceed.

**NEX-FS01-57:** shall, once initiated or confirmed by the user, indicate the swap is in process. (loading spinner)

**NEX-FS01-58:** shall, once confirmed by the user, submit the swap as a fill-or-kill market order to the Peerplays network.

**NEX-FS01-59:** shall indicate to the user if the swap was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS01-60:** shall, if the swap process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

### 8.5. Dashboard market listings function

The dashboard, in the context of market listings:

**NEX-FS01-61:** shall display a list of every trading pair available within the app.

**NEX-FS01-62:** shall display the following information for each trading pair, if available:

* both asset symbols (for example, `BTC/PPY`)
* the current exchange rate
* the 24 hour change in exchange rate (`+x.xx%` or `-x.xx%`)

**NEX-FS01-63:** shall display all information in real time (or as close to real time as feasible).

**NEX-FS01-64:** shall clearly indicate to the user if the 24 hour change in exchange rate is positive or negative, per trading pair, through the use of color, iconography, or symbols.

**NEX-FS01-65:** shall clearly indicate which trading pair is currently being hovered over by the user.

**NEX-FS01-66:** shall allow a user to click on a trading pair to load that trading pair in the market page.

## 9. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
