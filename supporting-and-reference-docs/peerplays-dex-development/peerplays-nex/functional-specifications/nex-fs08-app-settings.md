---
description: >-
  The Peerplays NEX application functional requirements specification for the
  app settings page.
---

# NEX-FS08 App Settings

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the app settings page from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`page`

## 3. Scope

This FS will describe the requirements and basic design for the app’s app settings page.

### 3.1. Components

Specific components and features covered in this FS include:

* app settings page layout
* general settings
* key management settings
* membership settings

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                                 |
| ---------- | --------------------------------------- |
| NEX-FS08-# | NEX App Requirement - App Settings Page |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

## 5. Context

The app settings provide a user the ability to adjust the design and function of the app. Here they’ll be able to turn notifications off and on, change the preferred currency display, choose their language, and even pick between light and dark modes. The app settings also allow users to manage their keys and buy a lifetime membership.

## 6. Design Wire-frames

The wire-frames listed below are meant to represent the app’s app settings page in various states. These are provided to assist in understanding of what features may look like or their potential use. Final designs may be vastly different from these images.

![](<../../../../.gitbook/assets/NEX-FS08 App Settings General.drawio.png>)

Figure 1. general settings

![](<../../../../.gitbook/assets/NEX-FS08 App Settings KeyMgmt.drawio.png>)

Figure 2. key management settings

![](<../../../../.gitbook/assets/NEX-FS08 App Settings Membership.drawio.png>)

Figure 3. membership settings

## 7. Requirements

Requirements specific to the items listed in this FS are as follows.

### 7.1. App Settings Page Layout

The app settings page layout:

**NEX-FS08-1:** shall be available for authenticated users within the application menu.

**NEX-FS08-2:** shall provide the user with navigation to the following pages:

* the app dashboard
* the market page
* the user profile page
* the user wallet
* the app settings
* the blockchain blocks page (if advanced mode is active)
* the voting (GPOS) page (if advanced mode is active)

**NEX-FS08-3:** shall provide menus for displaying the following components:

* general settings
* key management settings
* membership settings

**NEX-FS08-4:** shall use graphic design elements which adhere to Peerplays branding guidelines.

**NEX-FS08-5:** shall use graphic design elements which remain consistent throughout the app.

**NEX-FS08-6:** shall allow user input in relevant form fields to perform the functions of the related component.

**NEX-FS08-7:** shall perform input field validation and inform the user of acceptable form inputs.

**NEX-FS08-8:** shall provide the user with help and/or hint text to explain available options and input fields.

**NEX-FS08-9:** shall provide unobtrusive help text to the user about each function. This may take any of the following forms:

* tooltip when hovering an element (button, title, etc.)
* pop-up when clicking an element (icon, link, etc.)
* expanding panels or menus with help text
* one time tutorial flow for new users
* similar techniques for revealing content

**NEX-FS08-10:** shall indicate to the user when content is loading and avoid showing incorrect, outdated, or blank content.

### 7.2. General Settings

**NEX-FS08-11:** shall allow the user to set the following options:

* language
* currency
* UI Design
* allow transfers to account (yes / no)
* enable notifications (yes / no)
* select notifications
* wallet lock timeout

**NEX-FS08-12:** shall allow the user to select among the following languages, including but not limited to: (single select)

* English (US)
* English (UK)
* Chinese
* Hindi
* Russian
* Spanish

**NEX-FS08-13:** shall allow the user to select among the following UI designs, including but not limited to: (single select)

* light mode
* dark mode

**NEX-FS08-14:** shall allow the user to enable or disable notifications for the following specific events, including but not limited to: (multi-select)

* funds sent from account
* funds received to account
* account updated
* asset swap started
* asset swap filled
* asset swap canceled
* order created
* order filled
* order canceled
* order expired

**NEX-FS08-15:** shall allow the user to lock their wallet after one of the following time intervals, including but not limited to: (single select)

* 10 minutes
* 30 minutes
* 1 hour
* 8 hours
* 24 hours
* 1 week
* 1 month
* never

**NEX-FS08-16:** shall display a notice to the user when they have changed but not yet saved their general settings.

**NEX-FS08-17:** shall remove the unsaved changes notice when the user successfully saves their general settings.

**NEX-FS08-18:** shall allow the user to save their general settings with a user input control button.

**NEX-FS08-19:** shall display feedback to the user when they have successfully saved their general settings.

**NEX-FS08-20:** shall, upon saving the user’s general settings, make all the necessary changes to the UI and app functions to accommodate the user’s new settings (settings go into effect).

**NEX-FS08-21:** shall display the standard Peerplays faucet URL as a reference for the user’s convenience.

### 7.3. Key Management Settings

**NEX-FS08-22:** shall provide a password input field and the following options for generating a new public/private key pair: (multi-select)

* Owner keys
* Active keys
* Memo keys

**NEX-FS08-23:** shall allow the user to initiate the key generation with a user input control button.

**NEX-FS08-24:** shall, when generating a new key pair, display (obfuscated with the option to deobfuscate, or “unhide”) the newly generated private key with a function to copy the key to the user’s clipboard.

**NEX-FS08-25:** shall, when generating a new key pair, display a link to download the public/private key pair and a warning for the user to do so.

**NEX-FS08-26:** shall display the user’s public keys which are associated with their account with a function to copy the key to the user’s clipboard.

### 7.4. Membership Settings

**NEX-FS08-27:** shall display a description of the lifetime membership (upgraded account) feature.

**NEX-FS08-28:** shall display the current cost (in PPY) of upgrading an account to lifetime membership status.

**NEX-FS08-29:** shall allow the user to initiate the account upgrade function with a user input control button.

**NEX-FS08-30:** shall display a description of the current network fee distribution.

**NEX-FS08-31:** shall display the reviewer, registrar, and referrer accounts associated with the user’s account.

**NEX-FS08-32:** shall display the membership expiration date and time of the user’s account (or if the expiration is not applicable).

**NEX-FS08-33:** shall display the total amount of fees paid by the user’s account.

**NEX-FS08-34:** shall display a description of the fee distribution and maintenance interval.

**NEX-FS08-35:** shall display a description of the vesting requirements for fee allocations.

## 8. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
