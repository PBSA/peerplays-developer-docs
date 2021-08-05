---
description: DEX Functional Specifications - Login / Account Creation
---

# Login and Account Creation

## 1. Purpose

The purpose of this document is to outline functional specifications for the Peerplays Decentralized Exchange \(DEX\) relating to user login / logout and account creation from the user's perspective.

## 2. Scope

The DEX requires user accounts for the management of user activities. Account creation and login functions are foundational aspects of exchange systems.

Specific functions covered include:

* the create an account function
* the log in / out function
* system behavior based on authentication state

## 3. Process Overview

Two processes will be described here: account creation, and logging in / out.

### 3.1. Account Creation

To create an account:

1. A new user \(no DEX account\) navigates to the Peerplays DEX app.
2. User initiates account creation via the "Create an Account" button.
   1. User is presented a dialog box containing an account creation form.
   2. User enters their desired account name.
   3. User enters their desired password.
   4. User enters their password again in a confirmation field.
   5. User submits the form with a confirmation button.
3. The system validates the form submission.
4. The system creates the user account on the Peerplays chain using a faucet.
5. The user is now logged in and presented with the DEX dashboard.

### 3.2. Logging In / Out

From a logged out state:

1. A user \(with DEX account\) navigates to the Peerplays DEX app.
2. User initiates logging in via the "Log In" button.
   1. User is presented a dialog box containing a log in form.
   2. User enters their DEX account name.
   3. User enters their password.
   4. User \(optionally\) checks the "Remember me" checkbox.
   5. User submits the form with a Log In button.
3. The system logs the user in and pulls in their account details.
4. The user is now logged in and presented with the DEX dashboard.

From a logged in state:

1. A logged in user navigates to \(or is already in\) the Peerplays DEX app.
2. User initiates logging out via the Account Menu &gt; Logout button.
3. The system logs the user out.
4. The user is presented with the DEX front page.

## 4. Context

To perform certain functions in the exchange, an authenticated account is required. This allows the system to pull specific user data like account balances, transaction history, wallet addresses, and to track open orders. Accounts also allow the system to perform actions on the behalf of the user like making trades and creating wallets or addresses.

## 5. Requirements

Requirements specific to the items outlined in this functional specification are as follows.

### 5.1. DEX application front page

The DEX app's front page exists to facilitate creating new accounts and logging in of existing users.

The system:

* must display the front page to users who navigate to the app and are either logged out or don't have an account.
* must not display the front page to logged in users.

The front page:

* must provide the user the ability to initiate the Create an Account dialog.
* must provide the user the ability to initiate the login dialog.

### 5.2. Create an Account dialog

The Create an Account dialog:

* must provide the following fields.
  * Account name \(required field\)
  * Password \(required field\)
  * Confirm password \(required field\)
* must provide the following actions via buttons.
  * Cancel
  * Create
* must provide real-time field validation as per the following.
  * Required fields are complete.
  * Account name field should validate:
    * if the account name is already in use.
    * the format of the account name is acceptable per system requirements. \(starts with lowercase letter, includes a hyphen or number, length requirements, etc.\)
  * Password field should validate:
    * the format of the account name is acceptable per system requirements. \(length requirements, etc.\)
  * Confirm password field should validate:
    * the confirmed password is identical to the password.
* must inform the user that the account name they choose will be public and be the identifier for their transactions.

The system:

* must create an account with the information supplied in the Create an Account dialog upon submission via the Create button.
* must cancel the account creation and return the user to the front page when the Cancel button is clicked.
* upon successful account creation, the system must display the dashboard to the user.

### 5.3. Login dialog

The Login dialog:

* must provide the following fields.
  * Account name \(required field\)
  * Password \(required field\)
  * Remember me \(optional checkbox\)
* must provide the following actions via buttons.
  * Cancel
  * Log In
* must provide real-time field validation as per the following.
  * Required fields are complete.

The system:

* must authenticate and log in the user with the information supplied in the Login dialog upon submission via the Log In button.
* if the Remember me option is checked, upon submission, the system must save the account name to remember the user's log in options.
* must cancel the login and return the user to the front page when the Cancel button is clicked.
* upon successful authentication and login, the system must display the dashboard to the user.

### 5.4. Account menu

The account menu:

* must be available to logged in users only. \(must not be available to logged out / new users.\)
* must provide the following actions via buttons.
  * Logout
* the logout action may be nested into a deeper menu navigation within the account menu.

The system:

* must log the user out when the logout button is clicked.
* upon successful logout, the system must display the front page to the user.

### 5.5. Application menu and application header

The application menu:

* may be available to logged in or logged out / new users.
* for logged in users:
  * all options in the application menu must be available.
* for logged out / new users:
  * the dashboard option must not be available. \(greyed out, leads to front page, hidden\)
  * the My Assets option must not be available. \(greyed out, leads to front page, hidden\)
  * all other options may be available but don't provide any user account specific information.

The application header:

* must display the application menu.
* must display the Log In button for unauthenticated users.
* must display the account menu for authenticated users.

### 5.6. The system

* if an error occurs at any point \(account creation, login, logout\), the system must display meaningful error information to the user and provide them with actions they can take to attempt to resolve the error.

## 6. Glossary

**Application header** : The top bar of the application that is available on every page. It holds the application menu, search bar, and account menu.

**Application menu** : The hamburger menu on the far left within the application header. It holds the main navigation options for the application: Dashboard, My Assets, Exchange, Settings, Contacts, and Help.

**Account menu** : The far right menu within the application header. It holds the account specific navigation options and quick actions.

