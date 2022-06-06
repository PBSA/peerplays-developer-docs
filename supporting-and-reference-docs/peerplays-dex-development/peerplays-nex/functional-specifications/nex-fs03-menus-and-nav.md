---
description: >-
  The Peerplays NEX application functional requirements specification for the
  menus and navigation functions.
---

# NEX-FS03 Menus and Nav

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the menus and navigation functions from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`Page Fragment`

## 3. Scope

This FS will describe the requirements and basic design for the app’s menus and navigation functions.

### 3.1. Components

Specific components and features covered in this FS include:

* App Header
  * Notification Bell Menu
  * Profile Icon Menu
  * Kebab Nav Menu
* Page Navigation

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                             |
| ---------- | ----------------------------------- |
| NEX-FS03-# | NEX App Requirement - Menus and Nav |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

## 5. Context

The app’s navigation menus should provide a logical flow. They allow users to discover all the possible functions within the app and provide an easy means to access them. Navigation should also be made available within the app pages as a convenience to users. For example, links to the login page should be presented when a user tries to access a function that requires authentication. After the authentication, the user should be returned to what they were doing.

## 6. Design Wire-frames

Designs for the NEX app menus and navigation functions are available here:

[DEX Hifi - All tabs with Market](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

Specific to menus and navigation, see screens:

* [Adobe XD screens 22 and 23](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/3ac822eb-9643-482b-992e-a8cbf536b6ad/) for the Kebab Nav Menu
* [Adobe XD screens 35 - 37](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/ce60547b-e938-47ff-bab2-9133309c6e3c/) for the Notification Bell Menu
* [Adobe XD screens 38 - 41](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/5c27dd9e-9b7c-4437-b73c-34a9844c93a2/) for the Profile Icon Menu

## 7. Requirements

Requirements specific to the items listed in this FS are as follows.

### 7.1. App Header

The app header:

**NEX-FS03-1:** shall display the Peerplays logo as per design guidelines.

**NEX-FS03-2:** shall, for **unauthenticated** users, provide the kebab nav menu with the following navigation links:

* Login
* Create Account
* Dashboard
* Market
* Blocks

**NEX-FS03-3:** shall, for authenticated users, provide the notification bell menu with the following navigation links and items:

* Show only unread (toggle)
* Recent notification items (See NEX-FS04 Notifications for details)
* See all account activity (link)

**NEX-FS03-4:** shall, for authenticated users, provide the profile icon menu with the following navigation links and items:

* Username initial as an avatar icon
* Welcome message
* Username
* Graphical status display (See NEX-FS11 API Functions for details)
* See all account activity (link)
* Settings
* Logout

**NEX-FS03-5:** shall, for authenticated users, provide the kebab nav menu with the following navigation links and items:

* Dashboard
* Market
* Blocks
* Wallet
* Settings
* Advanced Settings (toggle)
* Voting (visible with Advanced Settings)
* Logout

**NEX-FS03-6:** shall display menu contents while hovering on the menu icon.

**NEX-FS03-7:** shall indicate to the user which menu item is hovered and/or focused using typical design techniques such as subtle animations, highlighting content, applying drop shadows or other styling.

**NEX-FS03-8:** shall use tab index and other universal design features to enable the use of keyboard navigation of the app menus and page fields. (See NEX-RS Requirements Specification about the use of Universal Design Principles in the app)

**NEX-FS03-9:** navigational menu items when activated by the user, shall load the related page within the app.

**NEX-FS03-10:** shall, when the Peerplays logo is clicked or otherwise activated by the user, load the app’s root homepage.

### 7.2. Notification Bell Menu

**NEX-FS03-11:** shall display a bell icon as the menu icon.

**NEX-FS03-12:** shall, if new notifications exist, display a notification bubble or badge on the bell icon.

**NEX-FS03-13:** shall display notifications as menu items as per NEX-FS04 Notifications specifies.

**NEX-FS03-14:** shall display only new notifications to the user if the “Show only unread” toggle is enabled. New notifications are notifications which have not yet been displayed in the menu to the user before.

**NEX-FS03-15:** shall display relevant icons for notification menu items.

### 7.3. Profile Icon Menu

**NEX-FS03-16:** shall, for **unauthenticated** users, display a generic avatar icon as the menu icon.

**NEX-FS03-17:** shall, for authenticated users, display the user’s username initial as an avatar icon as the menu icon.

**NEX-FS03-18:** shall display relevant icons for the settings (gear) and logout (power symbol) menu items.

### 7.4. Kebab Nav Menu

**NEX-FS03-19:** shall display a kebab (three vertical dots) icon as the menu icon.

**NEX-FS03-20:** shall display relevant icons for the following menu items:

* Dashboard (grid or mosaic)
* Market (chart)
* Wallet (wallet, currency, coins, etc.)
* Settings (gear)
* Advanced settings (toggle switch)
* Blocks (stacked shapes, chain links, boxes, etc.)
* Voting (ballot box)
* Logout (power symbol)

### 7.5. Page Navigation

**NEX-FS03-21:** shall, when an app feature or function requires a user to log in, temporarily redirect the user to the login page.

**NEX-FS03-22:** When finished with logging in, the app shall return the user to the page that initiated the login redirect.

**NEX-FS03-23:** Furthermore, if the user must create an account from this redirect, the app shall return the user to the page which initiated the redirect after the user creates an account.

**NEX-FS03-24:** shall load the login page when the user logs out.

## 8. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
