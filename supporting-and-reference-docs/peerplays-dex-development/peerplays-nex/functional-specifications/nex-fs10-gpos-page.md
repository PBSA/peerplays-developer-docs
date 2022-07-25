---
description: >-
  The Peerplays NEX application functional requirements specification for the
  GPOS page.
---

# NEX-FS10 GPOS Page

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the GPOS page from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`page`

## 3. Scope

This FS will describe the requirements and basic design for the app’s GPOS page.

### 3.1. Components

Specific components and features covered in this FS include:

* gpos page layout
* gpos dashboard
* gpos power up
* gpos power down
* witness voting
* SONs voting
* committee member voting
* proxy voting

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                         |
| ---------- | ------------------------------- |
| NEX-FS10-# | NEX App Requirement - GPOS Page |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

## 5. Context

The Gamified Proof of Stake (GPOS) page is for blockchain governance by voting for the various nodes in the network. Users need a simple and intuitive way to compare the candidates they can vote on. The GPOS consensus mechanism is unique and needs to be explained to the user so that they know what GPOS is, how it works, and how it can effect their voting power and available balance over time.

## 6. Design Wire-frames

The wire-frames listed below are meant to represent the app’s GPOS page in various states. These are provided to assist in understanding of what features may look like or their potential use. Final designs may be vastly different from these images.

![NEX-FS10 GPOS Page-GPOS.drawio.png](<../../../../.gitbook/assets/NEX-FS10 GPOS Page-GPOS.drawio.png>)

Figure 1. GPOS Page



![NEX-FS10 GPOS Page-GPOS Power Up.drawio.png](<../../../../.gitbook/assets/NEX-FS10 GPOS Page-GPOS Power Up.drawio.png>)

Figure 2. GPOS Power Up



![NEX-FS10 GPOS Page-GPOS Power Down.drawio.png](<../../../../.gitbook/assets/NEX-FS10 GPOS Page-GPOS Power Down.drawio.png>)

Figure 3. GPOS Power Down



![NEX-FS10 GPOS Page-Witness Vote.drawio.png](<../../../../.gitbook/assets/NEX-FS10 GPOS Page-Witness Vote.drawio.png>)

Figure 4. Witness Voting



![NEX-FS10 GPOS Page-SONs Vote.drawio.png](<../../../../.gitbook/assets/NEX-FS10 GPOS Page-SONs Vote.drawio.png>)

Figure 5. SON Voting



![NEX-FS10 GPOS Page-Committee Vote.drawio.png](<../../../../.gitbook/assets/NEX-FS10 GPOS Page-Committee Vote.drawio.png>)

Figure 6. Committee Member Voting



![NEX-FS10 GPOS Page-Proxy Vote.drawio.png](<../../../../.gitbook/assets/NEX-FS10 GPOS Page-Proxy Vote.drawio.png>)

Figure 7. Proxy Voting

## 7. Requirements

Requirements specific to the items listed in this FS are as follows.

### 7.1. GPOS Page Layout

The GPOS page layout:

**NEX-FS10-1:** shall be available for authenticated users within the application menu.

**NEX-FS10-2:** shall provide the user with navigation to the following pages:

* the app dashboard
* the market page
* the user profile page
* the user wallet
* the app settings
* the blockchain blocks page
* the voting (GPOS) page (if advanced mode is active)

**NEX-FS10-3:** shall provide menus for displaying the following components:

* GPOS dashboard
* Witness voting
* SONs voting
* Committee member voting
* Proxy voting

**NEX-FS10-4:** shall use graphic design elements which adhere to Peerplays branding guidelines.

**NEX-FS10-5:** shall use graphic design elements which remain consistent throughout the app.

**NEX-FS10-6:** shall allow user input in relevant form fields to perform the functions of the related component.

**NEX-FS10-7:** shall perform input field validation and inform the user of acceptable form inputs.

**NEX-FS10-8:** shall provide the user with help and/or hint text to explain available options and input fields.

**NEX-FS10-9:** shall provide unobtrusive help text to the user about each function. This may take any of the following forms:

* tooltip when hovering an element (button, title, etc.)
* pop-up when clicking an element (icon, link, etc.)
* expanding panels or menus with help text
* one time tutorial flow for new users
* similar techniques for revealing content

**NEX-FS10-10:** shall indicate to the user when content is loading and avoid showing incorrect, outdated, or blank content.

**NEX-FS10-11:** displayed lists shall provide pagination controls for large record sets requiring multiple pages.

**NEX-FS10-12:** displayed lists shall display information about their records in a tabular format.

**NEX-FS10-13:** displayed lists shall display a header row with column names.

**NEX-FS10-14:** displayed lists shall provide controls in the header to sort columns based on their values, if appropriate.

**NEX-FS10-15:** displayed lists shall display columns of a width appropriate for their values.

**NEX-FS10-16:** displayed lists shall highlight row which the user is hovering on.

**NEX-FS10-17:** displayed lists shall highlight row which the user has clicked on (focused).

### 7.2. GPOS Dashboard

**NEX-FS10-18:** shall display a description of GPOS to the user to satisfy the following goals:

* to educate the user about what GPOS is, how it works, and why it’s important to the network.
* to persuade the user to participate in GPOS consensus.

**NEX-FS10-19:** shall display the user’s current GPOS metrics including:

* GPOS balance
* Voting performance
* Qualified reward %
* Estimated rake reward %

**NEX-FS10-20:** shall display the user’s current available PPY balance with which they can Power Up to their GPOS balance.

**NEX-FS10-21:** shall allow users to navigate to the GPOS Power Up screen, GPOS Power Down screen, or to the witness voting screen via user input controls (buttons).

### 7.3. GPOS Power Up

**NEX-FS10-22:** shall display a description of GPOS Power Up to the user to satisfy the following goals:

* to educate the user about what GPOS Power Up is, how it works, and why it’s important to the network.
* to persuade the user to participate in GPOS consensus.

**NEX-FS10-23:** shall display a notice to the user which explains the pros, cons, and implications of participating in GPOS via Power Up. For example:

> “When you Power Up your PPY you are staking your PPY. This means you won't be able to transfer, spend, or otherwise use your PPY until you Power Down your PPY. Powered Up (staked) PPY will grant your account voting power equal to your balance of staked PPY. When Powering Down, or unstaking, your PPY will be returned to your account gradually over a 30 day period.”

**NEX-FS10-24:** shall display the user’s opening GPOS balance.

**NEX-FS10-25:** shall display the user’s current available PPY balance with which they can Power Up to their GPOS balance.

**NEX-FS10-26:** shall allow users to enter an amount of PPY they wish to deposit to their GPOS balance using a numerical user input control.

**NEX-FS10-27:** shall validate the number entered into the deposit field as follows:

* deposit cannot be 0.
* deposit cannot be negative.
* deposit must be a number.
* deposit must not be greater than available balance.

**NEX-FS10-28:** shall display the user’s new GPOS balance should they decide to vest the amount they entered into the deposit field.

**NEX-FS10-29:** shall provide a user control to submit the transaction to the network. (vest button)

**NEX-FS10-30:** shall provide a cancel button to cancel the Power Up and return the user to the GPOS dashboard screen.

**NEX-FS10-31:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS10-32:** shall, once initiated by the user, summarize the Power Up and ask the user for their confirmation to proceed.

**NEX-FS10-33:** shall, once initiated or confirmed by the user, indicate Power Up is in process. (loading spinner)

**NEX-FS10-34:** shall, once confirmed by the user, submit the transaction to the Peerplays network.

**NEX-FS10-35:** shall indicate to the user if the Power Up was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS10-36:** shall, if the Power Up initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS10-37:** shall, upon successful Power Up initiation, update the user’s GPOS and PPY balances.

### 7.4. GPOS Power Down

**NEX-FS10-38:** shall display a description of GPOS Power Down to the user to satisfy the following goals:

* to educate the user about what GPOS Power Down is, how it works, and why it’s important to the network.
* to persuade the user to participate in GPOS consensus.

**NEX-FS10-39:** shall display a notice to the user which explains the pros, cons, and implications of participating in GPOS via Power Down. For example:

> “When you Power Up your PPY you are staking your PPY. This means you won't be able to transfer, spend, or otherwise use your PPY until you Power Down your PPY. Powered Up (staked) PPY will grant your account voting power equal to your balance of staked PPY. When Powering Down, or unstaking, your PPY will be returned to your account gradually over a 30 day period.”

**NEX-FS10-40:** shall display the user’s opening GPOS balance.

**NEX-FS10-41:** shall display the user’s current available GPOS balance with which they can Power Down to their normal PPY balance.

**NEX-FS10-42:** shall allow users to enter an amount of PPY they wish to withdraw to their normal PPY balance using a numerical user input control.

**NEX-FS10-43:** shall validate the number entered into the withdraw field as follows:

* withdraw cannot be 0.
* withdraw cannot be negative.
* withdraw must be a number.
* withdraw must not be greater than available balance.

**NEX-FS10-44:** shall display the user’s new GPOS balance should they decide to withdraw the amount they entered into the withdraw field.

**NEX-FS10-45:** shall provide a user control to submit the transaction to the network. (withdraw button)

**NEX-FS10-46:** shall provide a cancel button to cancel the Power Down and return the user to the GPOS dashboard screen.

**NEX-FS10-47:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS10-48:** shall, once initiated by the user, summarize the Power Down and ask the user for their confirmation to proceed.

**NEX-FS10-49:** shall, once initiated or confirmed by the user, indicate Power Down is in process. (loading spinner)

**NEX-FS10-50:** shall, once confirmed by the user, submit the transaction to the Peerplays network.

**NEX-FS10-51:** shall indicate to the user if the Power Down was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS10-52:** shall, if the Power Down initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS10-53:** shall, upon successful Power Down initiation, update the user’s GPOS and PPY balances.

### 7.5. Witness Voting

**NEX-FS10-54:** shall display two tables (lists):

* list of pending changes to user voting
* list of witness accounts

**NEX-FS10-55:** shall include the following information in the pending changes table:

* pending change number
* witness account name
* currently active witness (true / false)
* witness proposal URL
* pending change description (voting to approve, voting to remove approval)
* user actions (cancel)

**NEX-FS10-56:** shall, as a user action for the pending changes list, allow the user to cancel a pending change.

**NEX-FS10-57:** shall, when the user cancels a pending change, remove the witness account from the pending changes list.

**NEX-FS10-58:** shall include the following information in the witness accounts table:

* witness rank (highest votes, length of time being active, and then least missed blocks.)
* witness name
* currently active witness (true / false)
* witness proposal URL
* total votes for the witness
* the user’s voting status (approved / not approved by user)
* user actions (add, remove, pending add, pending remove)

**NEX-FS10-59:** shall, as a user action for the witness accounts list, if the witness is currently not approved by the user, allow the user to add their approval (vote) for the witness.

**NEX-FS10-60:** shall, when the adding approval action is applied by the user, add the witness account to the pending changes list with the “voting to approve” or similar pending change.

**NEX-FS10-61:** shall, as a user action for the witness accounts list, if the witness is currently approved by the user, allow the user to remove their approval (remove vote) for the witness.

**NEX-FS10-62:** shall, when the removing approval action is applied by the user, add the witness account to the pending changes list with the “voting to remove approval” or similar pending change.

**NEX-FS10-63:** shall allow the user to click a witness name to navigate to the witness’s account page.

**NEX-FS10-64:** shall allow the user to click a witness URL to navigate to the witness’s proposal website (external link).

**NEX-FS10-65:** shall display the total votes amount in nominal value with five significant digits.

**NEX-FS10-66:** shall allow the user to sort the witness accounts list by its data fields.

**NEX-FS10-67:** shall allow the user to filter the witness accounts list by its data fields.

**NEX-FS10-68:** shall allow users to perform text search of keywords like witness names for the witness accounts list.

**NEX-FS10-69:** shall allow the user to download a copy of the witness accounts list, as it has been filtered and sorted, as either a PDF file or CSV file.

**NEX-FS10-70:** shall allow the user to cancel all pending changes.

**NEX-FS10-71:** shall allow the user to submit all pending changes to the network.

**NEX-FS10-72:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS10-73:** shall, once initiated by the user, summarize the pending changes to the user’s voting and ask the user for their confirmation to proceed.

**NEX-FS10-74:** shall, once initiated or confirmed by the user, indicate voting is in process. (loading spinner)

**NEX-FS10-75:** shall, once confirmed by the user, submit the transaction to the Peerplays network.

**NEX-FS10-76:** shall indicate to the user if the voting was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS10-77:** shall, if the voting initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS10-78:** shall, upon successful voting initiation, update the pending changes list and witness accounts list to reflect the user’s new votes.

### 7.6. SONs Voting

**NEX-FS10-79:** shall display two tables (lists):

* list of pending changes to user voting
* list of SON accounts

**NEX-FS10-80:** shall include the following information in the pending changes table:

* pending change number
* SON account name
* currently active SON (true / false)
* SON proposal URL
* pending change description (voting to approve, voting to remove approval)
* user actions (cancel)

**NEX-FS10-81:** shall, as a user action for the pending changes list, allow the user to cancel a pending change.

**NEX-FS10-82:** shall, when the user cancels a pending change, remove the SON account from the pending changes list.

**NEX-FS10-83:** shall include the following information in the SON accounts table:

* SON rank (highest votes, then length of time being active)
* SON name
* currently active SON (true / false)
* SON proposal URL
* total votes for the SON
* the user’s voting status (approved / not approved by user)
* user actions (add, remove, pending add, pending remove)

**NEX-FS10-84:** shall, as a user action for the SON accounts list, if the SON is currently not approved by the user, allow the user to add their approval (vote) for the SON.

**NEX-FS10-85:** shall, when the adding approval action is applied by the user, add the SON account to the pending changes list with the “voting to approve” or similar pending change.

**NEX-FS10-86:** shall, as a user action for the SON accounts list, if the SON is currently approved by the user, allow the user to remove their approval (remove vote) for the SON.

**NEX-FS10-87:** shall, when the removing approval action is applied by the user, add the SON account to the pending changes list with the “voting to remove approval” or similar pending change.

**NEX-FS10-88:** shall allow the user to click a SON name to navigate to the SON’s account page.

**NEX-FS10-89:** shall allow the user to click a SON URL to navigate to the SON’s proposal website (external link).

**NEX-FS10-90:** shall display the total votes amount in nominal value with five significant digits.

**NEX-FS10-91:** shall allow the user to sort the SON accounts list by its data fields.

**NEX-FS10-92:** shall allow the user to filter the SON accounts list by its data fields.

**NEX-FS10-93:** shall allow users to perform text search of keywords like SON names for the SON accounts list.

**NEX-FS10-94:** shall allow the user to download a copy of the SON accounts list, as it has been filtered and sorted, as either a PDF file or CSV file.

**NEX-FS10-95:** shall allow the user to cancel all pending changes.

**NEX-FS10-96:** shall allow the user to submit all pending changes to the network.

**NEX-FS10-97:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS10-98:** shall, once initiated by the user, summarize the pending changes to the user’s voting and ask the user for their confirmation to proceed.

**NEX-FS10-99:** shall, once initiated or confirmed by the user, indicate voting is in process. (loading spinner)

**NEX-FS10-100:** shall, once confirmed by the user, submit the transaction to the Peerplays network.

**NEX-FS10-101:** shall indicate to the user if the voting was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS10-102:** shall, if the voting initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS10-103:** shall, upon successful voting initiation, update the pending changes list and SON accounts list to reflect the user’s new votes.

### 7.7. Committee Member Voting

**NEX-FS10-104:** shall display two tables (lists):

* list of pending changes to user voting
* list of committee member accounts

**NEX-FS10-105:** shall include the following information in the pending changes table:

* pending change number
* committee member account name
* currently active committee member (true / false)
* committee member proposal URL
* pending change description (voting to approve, voting to remove approval)
* user actions (cancel)

**NEX-FS10-106:** shall, as a user action for the pending changes list, allow the user to cancel a pending change.

**NEX-FS10-107:** shall, when the user cancels a pending change, remove the committee member account from the pending changes list.

**NEX-FS10-108:** shall include the following information in the committee member accounts table:

* committee member rank (highest votes, then length of time being active)
* committee member name
* currently active committee member (true / false)
* committee member proposal URL
* total votes for the committee member
* the user’s voting status (approved / not approved by user)
* user actions (add, remove, pending add, pending remove)

**NEX-FS10-109:** shall, as a user action for the committee member accounts list, if the committee member is currently not approved by the user, allow the user to add their approval (vote) for the committee member.

**NEX-FS10-110:** shall, when the adding approval action is applied by the user, add the committee member account to the pending changes list with the “voting to approve” or similar pending change.

**NEX-FS10-111:** shall, as a user action for the committee member accounts list, if the committee member is currently approved by the user, allow the user to remove their approval (remove vote) for the committee member.

**NEX-FS10-112:** shall, when the removing approval action is applied by the user, add the committee member account to the pending changes list with the “voting to remove approval” or similar pending change.

**NEX-FS10-113:** shall allow the user to click a committee member name to navigate to the committee member’s account page.

**NEX-FS10-114:** shall allow the user to click a committee member URL to navigate to the committee member’s proposal website (external link).

**NEX-FS10-115:** shall display the total votes amount in nominal value with five significant digits.

**NEX-FS10-116:** shall allow the user to sort the committee member accounts list by its data fields.

**NEX-FS10-117:** shall allow the user to filter the committee member accounts list by its data fields.

**NEX-FS10-118:** shall allow users to perform text search of keywords like committee member names for the committee member accounts list.

**NEX-FS10-119:** shall allow the user to download a copy of the committee member accounts list, as it has been filtered and sorted, as either a PDF file or CSV file.

**NEX-FS10-120:** shall allow the user to cancel all pending changes.

**NEX-FS10-121:** shall allow the user to submit all pending changes to the network.

**NEX-FS10-122:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS10-123:** shall, once initiated by the user, summarize the pending changes to the user’s voting and ask the user for their confirmation to proceed.

**NEX-FS10-124:** shall, once initiated or confirmed by the user, indicate voting is in process. (loading spinner)

**NEX-FS10-125:** shall, once confirmed by the user, submit the transaction to the Peerplays network.

**NEX-FS10-126:** shall indicate to the user if the voting was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS10-127:** shall, if the voting initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS10-128:** shall, upon successful voting initiation, update the pending changes list and committee member accounts list to reflect the user’s new votes.

### 7.8. Proxy Voting

**NEX-FS10-129:** shall display two tables (lists):

* list of pending changes to user voting
* list of Peerplays accounts

**NEX-FS10-130:** shall display a notice to the user that it’s only possible to proxy one account at a time.

**NEX-FS10-131:** shall include the following information in the pending changes table:

* Peerplays account name
* number of witnesses voted for by this Peerplays account
* number of SONs voted for by this Peerplays account
* number of committee members voted for by this Peerplays account
* date when this Peerplays account last voted
* pending change description (proxy vote with this account, remove proxy)
* user actions (cancel)

**NEX-FS10-132:** shall, as a user action for the pending changes list, allow the user to cancel a pending change.

**NEX-FS10-133:** shall, when the user cancels a pending change, remove the Peerplays account from the pending changes list.

**NEX-FS10-134:** shall include the following information in the Peerplays accounts table:

* Peerplays account name
* number of witnesses voted for by this Peerplays account
* number of SONs voted for by this Peerplays account
* number of committee members voted for by this Peerplays account
* date when this Peerplays account last voted
* the user’s voting status (proxied / not proxied by user)
* user actions (add, remove, pending add, pending remove)

**NEX-FS10-135:** shall, as a user action for the Peerplays accounts list, if the Peerplays account is currently not proxied by the user, allow the user to add their account to votes by proxy.

**NEX-FS10-136:** shall, when the adding approval action is applied by the user, add the Peerplays account to the pending changes list with the “proxy vote with this account” or similar pending change.

**NEX-FS10-137:** shall, when the adding approval action is applied by the user, if the user is already proxying an account, add the current proxied account to the pending changes list as to remove its proxy. This is to keep only one account proxied and make it obvious to the user that one proxy is being swapped for the one they chose.

**NEX-FS10-138:** shall, as a user action for the Peerplays accounts list, if the Peerplays account is currently proxied by the user, allow the user to remove their proxy for the Peerplays account.

**NEX-FS10-139:** shall, when the removing proxy action is applied by the user, add the Peerplays account to the pending changes list with the “remove proxy” or similar pending change.

**NEX-FS10-140:** shall allow the user to click a Peerplays account name to navigate to the Peerplays account’s account page.

**NEX-FS10-141:** shall allow the user to sort the Peerplays accounts list by its data fields.

**NEX-FS10-142:** shall allow the user to filter the Peerplays accounts list by its data fields.

**NEX-FS10-143:** shall allow users to perform text search of keywords like Peerplays account names for the Peerplays accounts list.

**NEX-FS10-144:** shall allow the user to download a copy of the Peerplays accounts list, as it has been filtered and sorted, as either a PDF file or CSV file.

**NEX-FS10-145:** shall allow the user to cancel all pending changes.

**NEX-FS10-146:** shall allow the user to submit all pending changes to the network.

**NEX-FS10-147:** shall, when necessary, indicate to the user that the Peerplays network is not available for transactions.

**NEX-FS10-148:** shall, once initiated by the user, summarize the pending changes to the user’s proxy voting and ask the user for their confirmation to proceed.

**NEX-FS10-149:** shall, once initiated or confirmed by the user, indicate proxy voting is in process. (loading spinner)

**NEX-FS10-150:** shall, once confirmed by the user, submit the transaction to the Peerplays network.

**NEX-FS10-151:** shall indicate to the user if the proxy voting was successfully (or unsuccessfully) initiated. This could be indicated with a message modal, UI elements like color and icons, etc.

**NEX-FS10-152:** shall, if the proxy voting initiation process fails, explain to the user why the process failed and steps the user can take to fix the issue or learn more about the issue.

**NEX-FS10-153:** shall, upon successful proxy voting initiation, update the pending changes list and Peerplays accounts list to reflect the user’s new proxy voting.

## 8. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
