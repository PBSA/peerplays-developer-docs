---
description: >-
  The Peerplays NEX application functional requirements specification for the
  login and account creation functions.
---

# NEX-FS02 Login and Account Creation

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the login and account creation functions from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`process`

`Page Fragment`

## 3. Scope

This FS will describe the requirements and basic design for the app’s login and account creation functions.

### 3.1. Components

Specific components and features covered in this FS include:

* account creation
* login
* logout
* reauthentication

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                                          |
| ---------- | ------------------------------------------------ |
| NEX-FS02-# | NEX App Requirement - Login and Account Creation |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

## 5. Process Overviews

The processes which will be described here are as follows.

* account creation
* login
* logout
* reauthentication

### 5.1. Account Creation

1. The (unauthenticated) user navigates to the account creation page. This may have been from a variety of other pages which require the user to be logged in.
2. The app displays the account creation form. ([See Adobe XD screen 16](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/3adc0565-1624-476c-8f6e-41f938fb6a03/))
3. The user enters their desired username. Assuming the username is available for this example, an auto-generated master password will be created and displayed to the user. ([See Adobe XD screen 17](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/76130b29-b1ec-4ff7-b8ce-95de191a3bec/))
4. The user copies the master password into the confirmation field.
5. The user Downloads a generated file which contains the user’s master password and username.
6. The user clicks the “I understand Peerplays cannot recover my lost password” and “I have securely saved my password” check boxes.
7. The user clicks the “Create Account” button.
8. The app uses the info supplied by the user to create a new account via a Peerplays faucet.
9. The app stores the user session data in the local browser storage.
10. The app loads user specific data based on the local browser storage of user session data.

### 5.2. Login

1. The (unauthenticated) user navigates to the login page. This may have been from a variety of other pages which require the user to be logged in. ([Adobe XD screen 1](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/))
2. The app displays a form to the user which requires the user’s username and master password. ([Adobe XD screen 2](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/0baa5f25-065c-4827-bb0a-fb8b8d160676/) or [Adobe XD screen 18](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/aad4d765-854d-4d48-a79a-3e0758a8311e/))
3. The user enters their username and master password and clicks the “Log in” button.
4. The app validates the username and master password on the blockchain and, if successful, stores the user session data in the local browser storage.
5. The app loads user specific data based on the local browser storage of user session data.

### 5.3. Logout

1. The user clicks or otherwise activates the logout function. This may have been from a menu in the app header or an option on another page.
2. The app deletes the user session data from the local browser storage.
3. The app loads generic data for unauthenticated views of app pages.

### 5.4. Reauthentication

1. Logged in (authenticated) users will be presented with reauthentication modals when attempting to perform transactional operations on the chain. This includes actions like asset transfers, voting, and placing orders. The user must enter their master password to continue with the transaction.
2. The app validates the username from the user session data along with the entered master password. If successful, the transaction will continue.

## 6. Context

The foundation of app security is in providing user accounts. This allows for a personalized experience and gives users a space to manage their data. User accounts also allow the app and the blockchain to operate with transactional operations so the system can fulfill its purposes. The functions of account creation and login/logout are fundamental to the system.

## 7. Design Wire-frames

Designs for the NEX app login and account creation functions are available here:

[DEX Hifi - All tabs with Market](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

## 8. Requirements

Requirements specific to the items listed in this FS are as follows.

### 8.1. Account Creation

The account creation page:

**NEX-FS02-1:** shall provide user input controls for the following:

* username
* master password (auto-generated, read-only)
* reenter master password (for confirmation)
* Link to download master password file
* check box for “I understand Peerplays cannot recover my lost password” disclaimer
* check box for “I have securely saved my password” disclaimer
* button to submit form for account creation
* link to login page

**NEX-FS02-2:** shall perform field input validation which validates the following rules:

* (username) Input is not blank
* (username) Username is not taken
* (username) Input is lowercase and begins with a letter
* (username) Input either has no vowels, or has at least one hyphen (-), period (.), or number
* (username) Input ends with a letter or number
* (username) Input only contains lowercase letters, hyphens, periods, and numbers
* (username) Input does not have adjacent hyphens (--) or periods (..)
* (username) Input is between 3 and 63 characters long

**Note:** all of the above rules (except if the username is taken) are contained in this regex string:

```jsx
/^[a-z](?!.*([-.])\\1)((?=.*(-))|(?=.*([0-9])))[a-z0-9-.]{2,62}(?<![-.])$/g
```

* (master password confirmation) Input must match the auto-generated master password.
* (first disclaimer) Check box must be checked
* (second disclaimer) Check box must be checked

**NEX-FS02-3:** when the download master password file link is clicked, shall initiate a download to the user’s local browser of a password file named `Peerplays_account_recovery_<PeerplaysUsername>.txt` which contains the auto-generated master password.

**NEX-FS02-4:** shall not allow submitting the form until form validation is successful.

**NEX-FS02-5:** shall, once the validated form is submitted, use the info supplied by the user to create a new account via a Peerplays faucet, store the user session data in the local browser storage, and load either the page the user initially navigated from or the dashboard page.

### 8.2. Login

**NEX-FS02-6:** shall provide user input controls for the following:

* username
* master password
* button to submit form for login
* link to account creation page

**NEX-FS02-7:** shall perform field input validation which validates the following rules:

* (username) Input is not blank
* (master password) Input is not blank

**NEX-FS02-8:** shall not allow submitting the form until form validation is successful.

**NEX-FS02-9:** shall, once the validated form is submitted, use the info supplied by the user to validate the user credentials, store the user session data in the local browser storage, and load either the page the user initially navigated from or the dashboard page.

### 8.3. Logout

**NEX-FS02-10:** shall, upon user activation of the logout function, delete the user session data from the local browser storage and load the login page.

### 8.4. Reauthentication

**NEX-FS02-11:** shall, whenever the user attempts to perform an action that requires a transactional operation on the blockchain, challenge the user by presenting them a form requesting their master password.

**NEX-FS02-12:** shall provide user input controls for the following:

* master password
* button to submit form for reauthentication challenge

**NEX-FS02-13:** shall perform field input validation which validates the following rules:

* (master password) Input is not blank

**NEX-FS02-14:** shall not allow submitting the form until form validation is successful.

**NEX-FS02-15:** shall, once the validated form is submitted, use the info supplied by the user to validate the user credentials.

**NEX-FS02-16:** shall, upon successful validation of the reauthentication challenge, allow the transactional operations to proceed using the private keys derived from the user’s master password provided in the form.

## 9. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
