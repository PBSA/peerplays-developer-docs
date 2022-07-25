---
description: >-
  The Peerplays NEX application functional requirements specification for the
  blockchain page.
---

# NEX-FS09 Blockchain Page

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the blockchain page from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`page`

## 3. Scope

This FS will describe the requirements and basic design for the app’s blockchain page.

### 3.1. Components

Specific components and features covered in this FS include:

* blockchain page layout
* blockchain blocks
* blockchain block details
* blockchain block transaction details
* blockchain assets
* blockchain witnesses
* blockchain committee members
* blockchain SONs
* blockchain fees

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                               |
| ---------- | ------------------------------------- |
| NEX-FS09-# | NEX App Requirement - Blockchain Page |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

## 5. Context

The app must provide a way for users to view most of the blockchain transactional data. This keeps the information open and therefore decentralized. The blockchain page will act as a singular point of reference where users can browse, search, and even download reports of the blockchain data.

## 6. Design Wire-frames

The wire-frames listed below are meant to represent the app’s blockchain page in various states. These are provided to assist in understanding of what features may look like or their potential use. Final designs may be vastly different from these images.

![NEX-FS09 Blockchain Page-Blocks.drawio.png](<../../../../.gitbook/assets/NEX-FS09 Blockchain Page-Blocks.drawio.png>)

Figure 1. Blockchain page Blocks



![NEX-FS09 Blockchain Page-Block Detail.drawio.png](<../../../../.gitbook/assets/NEX-FS09 Blockchain Page-Block Detail.drawio.png>)

Figure 2. Block Details



![NEX-FS09 Blockchain Page-Transaction Detail.drawio.png](<../../../../.gitbook/assets/NEX-FS09 Blockchain Page-Transaction Detail.drawio.png>)

Figure 3. Transaction Details



![NEX-FS09 Blockchain Page-Assets.drawio.png](<../../../../.gitbook/assets/NEX-FS09 Blockchain Page-Assets.drawio.png>)

Figure 4. Assets



![NEX-FS09 Blockchain Page-Witnesses.drawio.png](<../../../../.gitbook/assets/NEX-FS09 Blockchain Page-Witnesses.drawio.png>)

Figure 5. Witnesses



![NEX-FS09 Blockchain Page-Committee.drawio.png](<../../../../.gitbook/assets/NEX-FS09 Blockchain Page-Committee.drawio.png>)

Figure 6. Committee Members



![NEX-FS09 Blockchain Page-SONs.drawio.png](<../../../../.gitbook/assets/NEX-FS09 Blockchain Page-SONs.drawio.png>)

Figure 7. SONs



![NEX-FS09 Blockchain Page-Fees.drawio.png](<../../../../.gitbook/assets/NEX-FS09 Blockchain Page-Fees.drawio.png>)

Figure 8. Fees

## 7. Requirements

Requirements specific to the items listed in this FS are as follows.

### 7.1. Blockchain Page Layout

The blockchain page layout:

**NEX-FS09-1:** shall be available for authenticated and unauthenticated users within the application menu.

**NEX-FS09-2:** shall provide the user with navigation to the following pages:

* the app dashboard
* the market page
* the user profile page
* the user wallet
* the app settings
* the blockchain blocks page
* the voting (GPOS) page (if advanced mode is active)

**NEX-FS09-3:** shall provide menus for displaying the following components:

* blockchain blocks
* blockchain assets
* blockchain Witnesses
* blockchain Committee members
* blockchain SONs
* blockchain fees

**NEX-FS09-4:** shall use graphic design elements which adhere to Peerplays branding guidelines.

**NEX-FS09-5:** shall use graphic design elements which remain consistent throughout the app.

**NEX-FS09-6:** shall allow user input in relevant form fields to perform the functions of the related component.

**NEX-FS09-7:** shall perform input field validation and inform the user of acceptable form inputs.

**NEX-FS09-8:** shall provide the user with help and/or hint text to explain available options and input fields.

**NEX-FS09-9:** shall provide unobtrusive help text to the user about each function. This may take any of the following forms:

* tooltip when hovering an element (button, title, etc.)
* pop-up when clicking an element (icon, link, etc.)
* expanding panels or menus with help text
* one time tutorial flow for new users
* similar techniques for revealing content

**NEX-FS09-10:** shall indicate to the user when content is loading and avoid showing incorrect, outdated, or blank content.

**NEX-FS09-11:** displayed lists shall provide pagination controls for large record sets requiring multiple pages.

**NEX-FS09-12:** displayed lists shall display information about their records in a tabular format.

**NEX-FS09-13:** displayed lists shall display a header row with column names.

**NEX-FS09-14:** displayed lists shall provide controls in the header to sort columns based on their values, if appropriate.

**NEX-FS09-15:** displayed lists shall display columns of a width appropriate for their values.

**NEX-FS09-16:** displayed lists shall highlight row which the user is hovering on.

**NEX-FS09-17:** displayed lists shall highlight row which the user has clicked on (focused).

### 7.2. Blockchain Blocks

**NEX-FS09-18:** shall display the following dynamic information about the blockchain, to be updated in real time:

* current block number
* last irreversible block number with difference from the current block number
* moving average confirmation time
* current total supply of PPY
* approximate value of 1 PPY (in user’s preferred currency)
* PPY market cap value (in user’s preferred currency)

**NEX-FS09-19:** shall, for each of the dynamic items listed above, display a chart or graph representing the change of these values for a reasonable time range (ex: last 24 hours, rolling).

**NEX-FS09-20:** shall, for each of the dynamic items listed above, display the label as well as information.

**NEX-FS09-21:** shall display a table (list) of recent blocks that have been produced on the blockchain.

**NEX-FS09-22:** shall include the following fields for each available recent block:

* block number
* block creation date and time
* name of the witness who produced the block
* number of transactions in the block

**NEX-FS09-23:** shall display date and time in the user’s local format.

**NEX-FS09-24:** shall allow the user to click a block number to navigate to a block details view.

**NEX-FS09-25:** shall allow the user to click a witness name to navigate to the witness’s account page.

**NEX-FS09-26:** shall allow the user to sort the list by its data fields.

**NEX-FS09-27:** shall allow the user to filter the list by its data fields.

**NEX-FS09-28:** shall allow users to perform text search of keywords like block numbers and witness names.

**NEX-FS09-29:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.3. Blockchain Block Details

**NEX-FS09-30:** shall provide pagination controls to navigate to the previous or next (if appropriate) block details view in block number order.

**NEX-FS09-31:** shall display the block number for the current block details view.

**NEX-FS09-32:** shall display the block creation time, in the user’s local date and time format, for the current block details view.

**NEX-FS09-33:** shall display the name and ID of the witness who created the block, which links to the witness’s account page.

**NEX-FS09-34:** shall display the number of transactions in the block.

**NEX-FS09-35:** shall display an expandable panel or menu which hides the advanced block details by default. This menu can be opened by the user to reveal the full details of the block which includes, but is not limited to, the following:

* block ID
* merkle root
* previous secret hash
* next secret hash
* signing key used to sign the block
* witness signature
* block extensions

**NEX-FS09-36:** shall display a table (list) of all the transactions contained in the block.

**NEX-FS09-37:** shall include the following fields for each available transaction within the block:

* transaction index
* transaction ID
* expiration date and time
* number of operations in the transaction
* reference block prefix
* reference block number
* number of transaction extensions

**NEX-FS09-38:** shall allow the user to click a transaction ID to navigate to a block transaction details view.

**NEX-FS09-39:** shall allow the user to sort the list by its data fields.

**NEX-FS09-40:** shall allow the user to filter the list by its data fields.

**NEX-FS09-41:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.4. Blockchain Block Transaction Details

**NEX-FS09-42:** shall provide pagination controls to navigate to the previous or next (if appropriate) block transaction details view in transaction index order.

**NEX-FS09-43:** shall display the block number for the current block transaction details view.

**NEX-FS09-44:** shall display the transaction expiration date and time, in the user’s local date and time format, for the current block transaction details view.

**NEX-FS09-45:** shall display the number of operations in the transaction.

**NEX-FS09-46:** shall display an expandable panel or menu which hides the advanced transaction details by default. This menu can be opened by the user to reveal the full details of the transaction which includes, but is not limited to, the following:

* transaction ID
* transaction memo
* reference block prefix
* reference block number
* transaction extensions
* signatures applied to the transaction

**NEX-FS09-47:** shall display a table (list) of all the operations contained in the transaction.

**NEX-FS09-48:** shall include the following fields for each available operation within the transaction:

* operation index
* operation ID
* operation type (name of the operation. ex: SON Heartbeat, transfer, or issue asset)
* fees
* operation details
* operation results

**NEX-FS09-49:** shall allow the user to sort the list by its data fields.

**NEX-FS09-50:** shall allow the user to filter the list by its data fields.

**NEX-FS09-51:** shall allow users to perform text search of keywords like operation type, or text within operation details.

**NEX-FS09-52:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.5. Blockchain Assets

Note: “Assets” in this context is referring to the blockchain objects beginning with object ID `1.3.x`. This is not intended to include NFT assets.

**NEX-FS09-53:** shall display the following dynamic information about the blockchain assets, to be updated in real time:

* number of assets on the chain

**NEX-FS09-54:** shall, for each of the dynamic items listed above, display a chart or graph representing the change of these values for a reasonable time range (ex: last 24 hours, rolling).

**NEX-FS09-55:** shall, for each of the dynamic items listed above, display the label as well as information.

**NEX-FS09-56:** shall display a table (list) of blockchain assets that have been created on the blockchain.

**NEX-FS09-57:** shall include the following fields for each available asset:

* asset ID (1.3.x)
* asset symbol (PPY, BTC, HIVE, etc.)
* asset name (Peerplays, Bitcoin, Hive, etc.)
* maximum supply
* asset precision
* asset issuer name

**NEX-FS09-58:** shall display the maximum supply amount in nominal value with significant digits appropriate to the asset.

**NEX-FS09-59:** shall allow the user to click an asset issuer name to navigate to the asset issuer’s account page.

**NEX-FS09-60:** shall allow the user to sort the list by its data fields.

**NEX-FS09-61:** shall allow the user to filter the list by its data fields.

**NEX-FS09-62:** shall allow users to perform text search of keywords like symbol, name, or issuer name.

**NEX-FS09-63:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.6. Blockchain Witnesses

**NEX-FS09-64:** shall display the following dynamic information about the blockchain witnesses, to be updated in real time:

* current number of active witnesses
* current block creation rewards per block
* estimated monthly rewards per witness
* remaining witness hourly budget
* date and time of the next vote update (maintenance period)
* current scheduled witness

**NEX-FS09-65:** shall, for each of the dynamic items listed above, display a chart or graph representing the change of these values for a reasonable time range (ex: last 24 hours, rolling) where appropriate.

**NEX-FS09-66:** shall, for each of the dynamic items listed above, display the label as well as information.

**NEX-FS09-67:** shall display a table (list) of witness accounts that have been created on the blockchain.

**NEX-FS09-68:** shall include the following fields for each witness account:

* witness rank (highest votes, length of time being active, and then least missed blocks.)
* witness name
* currently active (true / false)
* witness proposal URL
* last block number produced by the witness
* number of missed blocks
* total votes for the witness
* the witness’s public key

**NEX-FS09-69:** shall allow the user to click a witness name to navigate to the witness’s account page.

**NEX-FS09-70:** shall allow the user to click a witness URL to navigate to the witness’s proposal website (external link).

**NEX-FS09-71:** shall display the total votes amount in nominal value with five significant digits.

**NEX-FS09-72:** shall allow the user to sort the list by its data fields.

**NEX-FS09-73:** shall allow the user to filter the list by its data fields.

**NEX-FS09-74:** shall allow users to perform text search of keywords like witness names.

**NEX-FS09-75:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.7. Blockchain Committee Members

**NEX-FS09-76:** shall display the following dynamic information about the blockchain committee members, to be updated in real time:

* current number of active committee members

**NEX-FS09-77:** shall, for each of the dynamic items listed above, display a chart or graph representing the change of these values for a reasonable time range (ex: last 24 hours, rolling) where appropriate.

**NEX-FS09-78:** shall, for each of the dynamic items listed above, display the label as well as information.

**NEX-FS09-79:** shall display a table (list) of committee member accounts that have been created on the blockchain.

**NEX-FS09-80:** shall include the following fields for each committee member account:

* committee member rank (highest votes, then length of time being active)
* committee member name
* currently active (true / false)
* committee member proposal URL
* total votes for the committee member

**NEX-FS09-81:** shall allow the user to click a committee member name to navigate to the committee member’s account page.

**NEX-FS09-82:** shall allow the user to click a committee member URL to navigate to the committee member’s proposal website (external link).

**NEX-FS09-83:** shall display the total votes amount in nominal value with five significant digits.

**NEX-FS09-84:** shall allow the user to sort the list by its data fields.

**NEX-FS09-85:** shall allow the user to filter the list by its data fields.

**NEX-FS09-86:** shall allow users to perform text search of keywords like committee member names.

**NEX-FS09-87:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.8. Blockchain SONs

**NEX-FS09-88:** shall display the following dynamic information about the blockchain SONs, to be updated in real time:

* current number of active SONs
* SON daily budget
* date and time of the next vote update (maintenance period)

**NEX-FS09-89:** shall, for each of the dynamic items listed above, display a chart or graph representing the change of these values for a reasonable time range (ex: last 24 hours, rolling) where appropriate.

**NEX-FS09-90:** shall, for each of the dynamic items listed above, display the label as well as information.

**NEX-FS09-91:** shall display a table (list) of SON accounts that have been created on the blockchain.

**NEX-FS09-92:** shall include the following fields for each SON account:

* SON rank (highest votes, then length of time being active)
* SON name
* currently active (true / false)
* SON proposal URL
* total votes for the SON

**NEX-FS09-93:** shall allow the user to click a SON name to navigate to the SON’s account page.

**NEX-FS09-94:** shall allow the user to click a SON URL to navigate to the SON’s proposal website (external link).

**NEX-FS09-95:** shall display the total votes amount in nominal value with five significant digits.

**NEX-FS09-96:** shall allow the user to sort the list by its data fields.

**NEX-FS09-97:** shall allow the user to filter the list by its data fields.

**NEX-FS09-98:** shall allow users to perform text search of keywords like SON names.

**NEX-FS09-99:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.9. Blockchain Fees

**NEX-FS09-100:** shall display a table (list) of fees that have been created on the blockchain.

**NEX-FS09-101:** shall include the following fields for each fee:

* fee category (general, asset specific, account specific, etc.)
* blockchain operation
* fee type (regular, price per KByte, etc.)
* fee amount (0.01 PPY, Free, etc.)

**NEX-FS09-102:** shall allow the user to sort the list by its data fields.

**NEX-FS09-103:** shall allow the user to filter the list by its data fields.

**NEX-FS09-104:** shall allow users to perform text search of keywords like blockchain operation, category, or fee amount.

**NEX-FS09-105:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

## 8. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
