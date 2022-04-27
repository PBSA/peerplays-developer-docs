---
description: >-
  The Peerplays NFT Store application functional requirements specification for
  the browsing views of objects.
---

# APP FS08 Browse View

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NFT Store application (the “app”) relating to the object lists from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NFT Store [Requirements Specification](https://devs.peerplays.tech/supporting-and-reference-docs/nft-development/nft-store/nft-store-requirements-specification).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`Page`

`Page Fragment`

## 3. Scope

This FS will describe the requirements and basic design for the app’s object lists.

### 3.1. Components

Specific components and features covered in this FS include:

* Browsing Views for each object (Peers, Galleries, Collections, NFTs, and Activities)
  * Compact Lists
  * Roomy Lists
  * Grids
  * Mosaics
* Search, Filter, and Sort while Browsing
* Pagination Controls

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                                 |
| ---------- | --------------------------------------- |
| APP-FS08-# | App Component Requirement - Browse View |

**The keyword `shall` indicates a requirement statement.**

The keywords `may`, `could`, and `should` are not requirements but rather indicate items related to requirements that are worthy of consideration.

The following terms are used to describe specific users of the application:

* Unauthenticated (not logged in) users are known as `visitors`.
* Authenticated (logged in) users are known as `peers`.

The following terms are used to describe levels of user entitlement within the application:

* A `browser` is view only (except for account creation and logging in) and used for visitors.
* An `enjoyer` can interact with the market, including buying and optionally re-selling NFTs, but can’t make new NFTs.
* A `tenant` can create NFTs and sell them in addition to what the enjoyer can do.
* A `client` is an administrator level user with all entitlements.

## 5. Context

For a complete app, users must be able to view many related objects on one page. The way these objects are presented must be flexible to provide a high level of customization by the end user. Users have different needs depending on their goal. Some users want a dense and information rich display while others want a minimal or image focused display.

## 6. Design Wire-frames

The wire-frames listed below are meant to represent the app’s browsing views in various states. These are provided to assist in understanding of what features may look like or their potential use. Final designs may be vastly different from these images.

### 6.1. Browsing View Layout

![Figure 1. The layout of a typical browsing view.](<../../../../.gitbook/assets/APP-FS08-browse-view-Browse View.drawio.png>)

### 6.2. Peer Object Browsing Views

![](<../../../../.gitbook/assets/APP-FS08 Peer Roomy List 2.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Peer Roomy List 1.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Peer Compact List.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Peer Grid.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Peer Mosaic.drawio.png>)

### 6.3. Gallery Object Browsing Views

![](<../../../../.gitbook/assets/APP-FS08 Gallery Roomy List 1.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Gallery Roomy List 2.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Gallery Grid.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Gallery Mosaic.drawio.png>)

### 6.4. Collection Object Browsing Views

![](<../../../../.gitbook/assets/APP-FS08 Collection Roomy List 1.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Collection Roomy List 2.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Collection Grid.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Collection Mosaic.drawio.png>)

### 6.5. NFT Object Browsing Views

![](<../../../../.gitbook/assets/APP-FS08 NFT Roomy List 1.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 NFT Roomy List 2.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 NFT Grid.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 NFT Mosaic.drawio.png>)

### 6.6. Activity Object Browsing Views

![](<../../../../.gitbook/assets/APP-FS08 Activity Compact List.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Activity Roomy List 1.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Activity Roomy List 2.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Activity Grid.drawio.png>) ![](<../../../../.gitbook/assets/APP-FS08 Activity Mosaic.drawio.png>)

## 7. Requirements

Requirements specific to the items listed in this FS are as follows.

### 7.1. Compact Lists

The compact list browsing views:

**APP-FS08-1:** shall display information about its objects in a tabular format.

**APP-FS08-2:** shall display a header row with column names.

**APP-FS08-3:** shall provide controls in the header to sort columns based on their values, if appropriate.

**APP-FS08-4:** shall display columns of a width appropriate for their values.

**APP-FS08-5:** shall highlight row which the user is hovering on.

**APP-FS08-6:** shall highlight row which the user has clicked on (focused).

**APP-FS08-7:** shall provide a contextual menu icon while the user is hovering on the object record.

This context menu (for Peers, Galleries, Collections, and NFTs) shall contain the following options:

* follow this \[object]
* flag for moderation

This context menu (for Activities) shall contain the following options:

* follow this \[object]

If the user owns the object, the context menu (for Peers, Galleries, Collections, and NFTs) shall contain the following instead:

* edit \[object]
* delete \[object]

**APP-FS08-8:** shall display values with rich media where appropriate (images, notification bubbles, icons, tags, etc.)

**APP-FS08-9:** shall provide links within the table values for easy navigation and / or functions (follow this peer, etc.)

#### 7.1.1. Peers

Compact lists for Peer object records:

**APP-FS08-10:** shall include, but is not limited to, the following fields related to each object:

* avatar image
* public display name
* number of views
* number of follows
* number of NFTs in the Display Case
* number of galleries (and list of popular galleries)
* number of collections (and list of popular collections)
* number of NFTs on sale (or auction)
* number of recent activities

#### 7.1.2. Galleries

Compact lists for Gallery object records:

**APP-FS08-11:** shall include, but is not limited to, the following fields related to each object:

* gallery image
* gallery name
* gallery creator
* number of views
* number of follows
* number of collections (and list of popular collections)
* number of NFTs on sale (or auction)
* number of recent activities

#### 7.1.3. Collections

Compact lists for Collection object records:

**APP-FS08-12:** shall include, but is not limited to, the following fields related to each object:

* collection image
* collection name
* gallery name
* collection creator
* number of views
* number of follows
* number of NFTs on sale (or auction)
* number of recent activities

#### 7.1.4. NFTs

Compact lists for NFT object records:

**APP-FS08-13:** shall include, but is not limited to, the following fields related to each object:

* NFT image
* NFT name
* collection name
* NFT owner
* number of views
* number of follows
* list of tags (categorizations, traits, other NFT specific metadata)
* number of recent activities
* current price (sale) or current bid (auction)
* accepted currency
* user control to buy or place a bid
* price data (% change)
* volume data
* remaining time at auction

#### 7.1.5. Activities

Compact lists for Activity object records:

**APP-FS08-14:** shall include, but is not limited to, the following fields related to each object:

* date and time of activity
* activity name (tag)
* detailed activity description with relevant links
* transaction ID
* fee

### 7.2. Roomy Lists

The roomy list browsing views:

**APP-FS08-15:** shall display information about its objects in a multi line list format.

**APP-FS08-16:** shall display one or two columns as appropriate based on responsive design media queries (screen width).

**APP-FS08-17:** shall highlight object record which user is hovering on.

**APP-FS08-18:** shall highlight object record which user has clicked on (focused).

**APP-FS08-19:** shall provide a contextual menu icon while the user is hovering on the object record.

This context menu (for Peers, Galleries, Collections, and NFTs) shall contain the following options:

* follow this \[object]
* flag for moderation

This context menu (for Activities) shall contain the following options:

* follow this \[object]

If the user owns the object, the context menu (for Peers, Galleries, Collections, and NFTs) shall contain the following instead:

* edit \[object]
* delete \[object]

**APP-FS08-20:** shall display values with rich media where appropriate (images, notification bubbles, icons, tags, etc.)

**APP-FS08-21:** shall provide links within the object record values for easy navigation and / or functions (follow this peer, etc.)

#### 7.2.1. Peers

Roomy lists for Peer object records:

**APP-FS08-22:** shall include, but is not limited to, the following fields related to each object:

* avatar image
* public display name
* social media links
* public profile description (if available)
* gallery names with links (if available)
* collection names with links (if available)
* number of views
* number of follows
* number of NFTs in the Display Case
* number of galleries
* number of collections
* number of NFTs on sale (or auction)
* number of recent activities

#### 7.2.2. Galleries

Roomy lists for Gallery object records:

**APP-FS08-23:** shall include, but is not limited to, the following fields related to each object:

* gallery image
* gallery name
* gallery creator
* gallery description (if available)
* sample of collection images (if available)
* list of tags (categorizations, traits, other metadata)
* number of views
* number of follows
* number of collections
* number of NFTs on sale (or auction)
* number of recent activities

#### 7.2.3. Collections

Roomy lists for Collection object records:

**APP-FS08-24:** shall include, but is not limited to, the following fields related to each object:

* collection image
* collection name
* gallery name
* collection creator
* collection description (if available)
* sample of NFT images (if available)
* list of tags (categorizations, traits, other metadata)
* number of views
* number of follows
* number of NFTs on sale (or auction)
* number of recent activities

#### 7.2.4. NFTs

Roomy lists for NFT object records:

**APP-FS08-25:** shall include, but is not limited to, the following fields related to each object:

* NFT image
* NFT name
* collection name
* NFT owner
* NFT description (if available)
* list of tags (categorizations, traits, other metadata)
* number of views
* number of follows
* number of recent activities
* current price (sale) or current bid (auction)
* accepted currency
* user control to buy or place a bid
* price data (% change)
* volume data
* remaining time at auction

#### 7.2.5. Activities

Roomy lists for Activity object records:

**APP-FS08-26:** shall include, but is not limited to, the following fields related to each object:

* date and time of activity
* small activity graphical diagram (see section 6.6 above for examples)
* activity name (tag)
* detailed activity description with relevant links
* transaction ID
* fee

### 7.3. Grids

The grid browsing views:

**APP-FS08-27:** shall display information about its objects in a grid (card) format.

**APP-FS08-28:** shall display one to six columns as appropriate based on responsive design media queries (screen width).

**APP-FS08-29:** shall highlight object record which user is hovering on.

**APP-FS08-30:** shall highlight object record which user has clicked on (focused).

**APP-FS08-31:** shall provide a contextual menu icon while the user is hovering on the object record.

This context menu (for Peers, Galleries, Collections, and NFTs) shall contain the following options:

* follow this \[object]
* flag for moderation

This context menu (for Activities) shall contain the following options:

* follow this \[object]

If the user owns the object, the context menu (for Peers, Galleries, Collections, and NFTs) shall contain the following instead:

* edit \[object]
* delete \[object]

**APP-FS08-32:** shall display additional information, if available, while the user is hovering and / or focused on the object record.

**APP-FS08-33:** shall display values with rich media where appropriate (images, notification bubbles, icons, tags, etc.)

**APP-FS08-34:** shall provide links within the object record values for easy navigation and / or functions (follow this peer, etc.)

#### 7.3.1. Peers

Grids for Peer object records:

**APP-FS08-35:** shall include, but is not limited to, the following fields related to each object:

* avatar image
* public display name
* number of NFTs in the Display Case
* number of galleries
* number of collections
* number of NFTs on sale (or auction)

Additionally, when hovered and / or focused, include:

* social media links

#### 7.3.2. Galleries

Grids for Gallery object records:

**APP-FS08-36:** shall include, but is not limited to, the following fields related to each object:

* gallery image
* gallery creator avatar image
* gallery name
* gallery creator name
* gallery description (if available)

Additionally, when hovered and / or focused, include:

* list of tags (categorizations, traits, other metadata)

#### 7.3.3. Collections

Grids for Collection object records:

**APP-FS08-37:** shall include, but is not limited to, the following fields related to each object:

* collection image
* collection name
* gallery name
* collection creator
* collection description (if available)
* number of NFTs on sale (or auction)

Additionally, when hovered and / or focused, include:

* list of tags (categorizations, traits, other metadata)

#### 7.3.4. NFTs

Grids for NFT object records:

**APP-FS08-38:** shall include, but is not limited to, the following fields related to each object:

* NFT image
* NFT name
* collection name
* remaining time at auction
* current price (sale) or current bid (auction)
* accepted currency
* user control to buy or place a bid

Additionally, when hovered and / or focused, include:

* NFT owner
* list of tags (categorizations, traits, other metadata)

#### 7.3.5. Activities

Grids for Activity object records:

**APP-FS08-39:** shall include, but is not limited to, the following fields related to each object:

* date and time of activity
* medium activity graphical diagram (see section 6.6 above for examples)
* activity name (tag)
* detailed activity description with relevant links
* transaction ID

### 7.4. Mosaics

The mosaic browsing views:

**APP-FS08-40:** shall display information about its objects in a mosaic tile (tessellation) format.

**APP-FS08-41:** shall display one to six columns as appropriate based on responsive design media queries (screen width).

**APP-FS08-42:** shall highlight object record which user is hovering on.

**APP-FS08-43:** shall highlight object record which user has clicked on (focused).

**APP-FS08-44:** shall provide a contextual menu icon while the user is hovering and / or focused on the object record.

This context menu (for Peers, Galleries, Collections, and NFTs) shall contain the following options:

* follow this \[object]
* flag for moderation

This context menu (for Activities) shall contain the following options:

* follow this \[object]

If the user owns the object, the context menu (for Peers, Galleries, Collections, and NFTs) shall contain the following instead:

* edit \[object]
* delete \[object]

**APP-FS08-45:** shall display additional information, if available, while the user is hovering and / or focused on the object record.

**APP-FS08-46:** shall display values with rich media where appropriate (images, notification bubbles, icons, tags, etc.) while the user is hovering and / or focused on the object record.

**APP-FS08-47:** shall provide links within the object record values for easy navigation and / or functions (follow this peer, etc.)

#### 7.4.1. Peers

Mosaics for Peer object records:

**APP-FS08-48:** shall include, but is not limited to, the following fields related to each object:

* avatar image

Additionally, when hovered and / or focused, include:

* public display name
* number of galleries
* number of collections
* number of NFTs on sale (or auction)

#### 7.4.2. Galleries

Mosaics for Gallery object records:

**APP-FS08-49:** shall include, but is not limited to, the following fields related to each object:

* gallery image

Additionally, when hovered and / or focused, include:

* gallery name
* gallery creator name
* list of tags (categorizations, traits, other metadata)

#### 7.4.3. Collections

Mosaics for Collection object records:

**APP-FS08-50:** shall include, but is not limited to, the following fields related to each object:

* collection image

Additionally, when hovered and / or focused, include:

* collection name
* gallery name
* collection creator
* number of NFTs on sale (or auction)
* list of tags (categorizations, traits, other metadata)

#### 7.4.4. NFTs

Mosaics for NFT object records:

**APP-FS08-51:** shall include, but is not limited to, the following fields related to each object:

* NFT image

Additionally, when hovered and / or focused, include:

* NFT name
* remaining time at auction
* current price (sale) or current bid (auction)
* accepted currency
* user control to buy or place a bid
* list of tags (categorizations, traits, other metadata)

#### 7.4.5. Activities

Mosaics for Activity object records:

**APP-FS08-52:** shall include, but is not limited to, the following fields related to each object:

* relative time of activity
* large activity graphical diagram (see section 6.6 above for examples)
* activity name (tag)
* transaction ID

### 7.5. Search, Filter, and Sort while Browsing

The browsing view, in the context of searching, filtering, sorting, and display:

**APP-FS08-53:** shall allow the user to hide and unhide the search menu.

**APP-FS08-54:** shall display the active text search query and active filters to the user.

**APP-FS08-55:** shall allow the user to cancel any individual active text search query or filter.

**APP-FS08-56:** shall allow the user to cancel all active text search queries and filters with one “clear all“ (or similar) function.

**APP-FS08-57:** shall provide a text search control with all standard search control features. (See APP-FS25 Search Functions)

**APP-FS08-58:** shall provide filter options to the user with the following specifications:

* Filter options are grouped into expanding menus.
* Filter options are based on available object metadata from within the subset of currently queried and filtered objects. (drill down filtering)
* Filter option controls can be ranged sliders, check boxes, radio boxes, combo boxes, drop down lists, or any other control that best fits the metadata values.

**APP-FS08-59:** shall provide the following display options:

* Sorting
* Display type
* Number of results returned per page

**APP-FS08-60:** shall allow sorting of the returned object records based on available object metadata.

**APP-FS08-61:** shall allow sorting via ascending and descending values.

**APP-FS08-62:** shall allow the selection of the following display types:

* Compact List
* Roomy List
* Grid
* Mosaic

**APP-FS08-63:** shall allow the user to select the number of object records to display per page (via pagination) from a list of sensible breakpoints, such as:

* 24, 48, 96, 192, and 384 (if 6 column display)
* 20, 40, 80, 160, and 320 (if 5 column display)
* 12, 36, 72, 144, and 288 (if 1, 2, 3, or 4 column display)

**APP-FS08-64:** shall allow the user, as an alternative to results per page, to enable an infinite scroll option to load more results when scrolling to the end of the current results.

### 7.6. Pagination Controls

The browsing view, in the context of pagination controls:

**APP-FS08-65:** shall provide the following abilities:

* View the next page of results, if available
* View the previous page of results, if available
* Display the total number of result pages
* Display the current page number
* Jump to a specified page number
* Display the total number of object records in the results

**APP-FS08-66:** shall not display pagination controls if infinite scroll is enabled. Instead, the page shall indicate when more records are loading or the end of the results have been reached.

### 8. Appendix A: Glossary

| Term   | Meaning                    |
| ------ | -------------------------- |
| RS     | Requirements Specification |
| FS     | Functional Specification   |
| NFT(s) | Non-Fungible Token(s)      |
| UI     | User Interface             |
| UX     | User Experience            |
