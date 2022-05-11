---
description: >-
  The Peerplays NEX application functional requirements specification for the
  notification functions.
---

# NEX-FS04 Notifications

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NEX application (the “app”) relating to the notification functions from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NEX Requirements Specification (NEX-RS).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`Page Fragment`

## 3. Scope

This FS will describe the requirements and basic design for the app’s notification functions.

### 3.1. Components

Specific components and features covered in this FS include:

* Notifications in pages
* Notifications in menus

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                             |
| ---------- | ----------------------------------- |
| NEX-FS04-# | NEX App Requirement - Notifications |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

## 5. Context

Notifications provide users with timely and important details about the status of their account. These brief messages are easy for users to digest so they can absorb the information and get back to their tasks quickly.

## 6. Design Wire-frames

Designs for the NEX app notification functions are available here:

[DEX Hifi - All tabs with Market](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/)

Specific to menus and navigation, see screens:

* [Adobe XD screen 34](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/d4d8c3ec-2317-4641-94aa-372b156ae4dd/) for the Notifications in pages
* [Adobe XD screens 35 - 37](https://xd.adobe.com/view/84e727ac-fe5c-4c6b-9dd2-d668cec13fc9-d1b4/screen/ce60547b-e938-47ff-bab2-9133309c6e3c/) for the Notifications in menus

## 7. Requirements

Requirements specific to the items listed in this FS are as follows.

### 7.1. Notifications

**NEX-FS04-1:** shall be displayed as “unread” when new activities occur, as close to real-time as feasible.

### 7.2. Notifications in Pages

The viewing all activity page:

**NEX-FS04-2:** shall display a table of all activity related to the user’s account.

The all activity table:

**NEX-FS04-3:** shall display a reasonable number of records for the given display (desktop, mobile, etc.). Additional records shall be accessible via table pagination controls.

**NEX-FS04-4:** shall display the following columns of data per record:

* Data and time (local)
* Activity type
* Detailed description of the activity
* Transaction ID
* Fee (in PPY)

**NEX-FS04-5:** shall display date and time information in the user’s local time and format.

**NEX-FS04-6:** shall display activity type as the operation relevant to the activity record. For example: Transfer, Account Creation, etc.

**NEX-FS04-7:** shall display a detailed description that explains what happened in the given activity. For example: “hiltos1 sent 1.000 BTC to m0rris0n” where “hiltos1” and “m0rris0n” are user accounts.

**NEX-FS04-8:** shall, in the detailed descriptions, provide links to any included user accounts.

**NEX-FS04-9:** shall sort the table in descending order based on date and time (newest first) by default.

**NEX-FS04-10:** shall allow users to sort the date and time, activity type, transaction ID, and Fee columns in ascending or descending order.

**NEX-FS04-11:** shall allow users to filter the date and time, activity type, transaction ID, and Fee columns as follows:

* Date and Time (Date and Time range)
* Activity Type (multi-select combo box of all activity types)
* Transaction ID (ID number range)
* Fee (Fee range)

**NEX-FS04-12:** shall allow users to perform text search of keywords like account names, assets, amounts, and transaction IDs.

### 7.3. Notifications in Menus

The notification bell menu:

**NEX-FS04-13:** shall display a list of all activity related to the user’s account, limited to the last 30 days of activity.

**NEX-FS04-14:** shall display a reasonable number of records for the given display (desktop, mobile, etc.). Additional records shall be accessible via table pagination controls or scrolling.

**NEX-FS04-15:** shall divide the list items by date.

**NEX-FS04-16:** shall display the current day’s activities in a “Today” group.

**NEX-FS04-17:** shall display the previous day’s activities in a “Yesterday” group.

**NEX-FS04-18:** shall display older activities in groups labeled by their date. For example: “August 12, 2021”

**NEX-FS04-19:** shall indicate “read” vs. “unread” notifications with icons, colors, or other style elements.

**NEX-FS04-20:** shall display a detailed description that explains what happened in the given activity. For example: “You sent 1.000 BTC to m0rris0n” where “m0rris0n” is a user account.

**NEX-FS04-21:** shall, in the detailed descriptions, provide links to any included user accounts.

**NEX-FS04-22:** shall sort the list in descending order by date and time (newest first).

## 8. Appendix A: Glossary

| Term | Meaning                    |
| ---- | -------------------------- |
| RS   | Requirements Specification |
| FS   | Functional Specification   |
| UI   | User Interface             |
