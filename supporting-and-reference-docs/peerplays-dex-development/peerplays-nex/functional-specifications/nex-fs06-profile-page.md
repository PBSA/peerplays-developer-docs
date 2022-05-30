---
description: >-
  The Peerplays NEX application functional requirements specification for the
  profile page.
---

# NEX-FS06 Profile Page

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the profile page from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`page`

## 3. Scope

This FS will describe the requirements and basic design for the app’s profile page.

### 3.1. Components

Specific components and features covered in this FS include:

* profile page layout
* orders list
* activity list
* notifications list

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                            |
| ---------- | ---------------------------------- |
| NEX-FS06-# | NEX App Requirement - Profile Page |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

In a trading pair like `BTC/PPY` the first asset (`BTC`) is known as the `base` asset. The second asset (`PPY`) is known as the `quote` asset. The terms base and quote will be used throughout this FS.

## 5. Context

The profile page of the app acts like a user’s personal dashboard. It should be used to display the user’s open orders, their order history, all account activity, and as the place to view and manage their notifications. The profile page also provides a quick navigational menu used to reach all of the user’s more personal pages such as their wallet, app settings, and voting page.

## 6. Design Wire-frames

The wire-frames listed below are meant to represent the app’s profile page in various states. These are provided to assist in understanding of what features may look like or their potential use. Final designs may be vastly different from these images.

![](<../../../../.gitbook/assets/NEX-FS06 Profile Page Orders.drawio.png>)

Figure 1. Open and historical orders on the profile page.

![](<../../../../.gitbook/assets/NEX-FS06 Profile Page Activity.drawio.png>)

Figure 2. All user activity listed on the profile page.

![](<../../../../.gitbook/assets/NEX-FS06 Profile Page Notifications.drawio.png>)

Figure 3. Notifications listed on the profile page.

## 7. Requirements

Requirements specific to the items listed in this FS are as follows.

### 7.1. Profile Page Layout

The profile page layout:

**NEX-FS06-1:** shall be available for authenticated users within the application menu.

**NEX-FS06-2:** shall provide the user with navigation to the following pages:

* the app dashboard
* the market page
* the user profile page
* the user wallet
* the app settings
* the blockchain blocks page (if advanced mode is active)
* the voting (GPOS) page (if advanced mode is active)

**NEX-FS06-3:** shall provide menus for displaying the following components:

* user’s open orders
* user’s order history
* user’s activity list
* user’s notifications list

**NEX-FS06-4:** shall use graphic design elements which adhere to Peerplays branding guidelines.

**NEX-FS06-5:** shall use graphic design elements which remain consistent throughout the app.

**NEX-FS06-6:** shall allow user input in relevant form fields to perform the functions of the related component.

**NEX-FS06-7:** shall perform input field validation and inform the user of acceptable form inputs.

**NEX-FS06-8:** shall provide the user with help and/or hint text to explain available options and input fields.

**NEX-FS06-9:** shall provide unobtrusive help text to the user about each function. This may take any of the following forms:

* tooltip when hovering an element (button, title, etc.)
* pop-up when clicking an element (icon, link, etc.)
* expanding panels or menus with help text
* one time tutorial flow for new users
* similar techniques for revealing content

**NEX-FS06-10:** shall indicate to the user when content is loading and avoid showing incorrect, outdated, or blank content.

**NEX-FS06-11:** displayed lists shall provide pagination controls for large record sets requiring multiple pages.

**NEX-FS06-12:** displayed lists shall display information about their records in a tabular format.

**NEX-FS06-13:** displayed lists shall display a header row with column names.

**NEX-FS06-14:** displayed lists shall provide controls in the header to sort columns based on their values, if appropriate.

**NEX-FS06-15:** displayed lists shall display columns of a width appropriate for their values.

**NEX-FS06-16:** displayed lists shall highlight row which the user is hovering on.

**NEX-FS06-17:** displayed lists shall highlight row which the user has clicked on (focused).

### 7.2. Orders List

#### 7.2.1. Open Orders

The profile page, in the context of the open orders list:

**NEX-FS06-18:** shall include all of the user's currently open orders with real-time data updates.

**NEX-FS06-19:** shall include the following data fields:

* the date and time each order was opened.
* the market pair
* the order type (limit, stop-limit, etc.)
* if the order is a buy or sell ("side")
* the order price (quote asset)
* the total order quantity (base asset)
* the filled order quantity value (base asset) and percentage of the order
* the total price (quote asset)
* the incurred fees
* the order status (open, partially filled, pending if stop-limit stop price hasn't been met)
* the expiry time (if applicable)
* may display a separate list of filled quantities of a partially filled order.

**NEX-FS06-20:** shall allow the user to sort the list by its data fields.

**NEX-FS06-21:** shall allow the user to filter the list by its data fields.

**NEX-FS06-22:** shall allow the user to cancel any open order, even if partially filled.

* user control for this may be a button or context menu item.

**NEX-FS06-23:** shall allow the user to open and close the list (expandable menu).

#### 7.2.2. Order History

The profile page, in the context of the order history list:

**NEX-FS06-24:** shall include all of the user's completed trade activities with real-time data updates.

**NEX-FS06-25:** shall include the following data fields:

* the date and time each order was opened.
* the date and time each trade was completed (filled, partially filled, canceled).
* the market pair
* the order type (market, limit, stop-limit, etc.)
* if the order was a buy or sell ("side")
* the order price (quote asset)
* the filled order quantity (base asset)
* the total price (quote asset)
* the incurred fees
* the order status (partially filled, filled, canceled by user, canceled by system, expired)
* may display a separate list of filled quantities of a partially filled order.

**NEX-FS06-26:** shall allow the user to sort the list by its data fields.

**NEX-FS06-27:** shall allow the user to filter the list by its data fields.

**NEX-FS06-28:** shall allow the user to open and close the list (expandable menu).

**NEX-FS06-29:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.3. Activity List

The profile page, in the context of the activity list:

**NEX-FS06-30:** shall include, but is not limited to, the following fields related to each record:

* date and time of activity
* activity type (tag)
* detailed activity description with relevant links (usernames)
* transaction ID
* fee

**NEX-FS06-31:** shall allow the user to sort the list by its data fields.

**NEX-FS06-32:** shall allow the user to filter the list by its data fields.

**NEX-FS06-33:** shall allow users to perform text search of keywords like account names, assets, amounts, and transaction IDs.

**NEX-FS06-34:** shall allow the user to download a copy of the list, as it has been filtered and sorted, as either a PDF file or CSV file.

### 7.4. Notifications List

These requirements are **in addition** to those found in [`NEX-FS04 Notifications`](nex-fs04-notifications.md#7.-requirements).

The profile page, in the context of the notifications list:

**NEX-FS06-35:** shall display a status field which represents if the notification is “Read” or “Unread”.

**NEX-FS06-36:** shall allow the user to mark a notification as read or unread.

**NEX-FS06-37:** shall display a notification bubble in the navigation tab indicating how many unread notifications exist, except when disabled in the app settings.

## 8. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
