---
description: >-
  The Peerplays NFT Store application functional requirements specification for
  the app home page.
---

# APP-FS06 App Home Page

## 1. Purpose

The purpose of this functional specification (FS) document is to detail functional requirements for the Peerplays NFT Store application (the “app”) relating to the app home page from a business and user perspective.

## 2. Document Tracking

### 2.1. Parent Document

This document is a child document of the NFT Store [Requirements Specification](https://devs.peerplays.tech/supporting-and-reference-docs/nft-development/nft-store/nft-store-requirements-specification).

### 2.2. Categorization

This document relates to the following tags.

`App Component`

`Page`

## 3. Scope

This FS will describe the requirements and basic design for the app’s home page.

### 3.1. Components

Specific components and features covered in this FS include:

* Content sections
* Key design characteristics

## 4. Document Conventions

For the purpose of traceability, the following code(s) will be used in this functional specification:

| Code       | Meaning                                   |
| ---------- | ----------------------------------------- |
| APP-FS06-# | App Component Requirement - App Home Page |

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

The app home page is the first page a user will see when visiting the NFT Store. It’s important to provide the best first impression possible as this will drive user engagement. The home page should portray a story to the user. It communicates the brand message and gives an opportunity to entice users to interact with the app. From a design standpoint, the home page is important for `SEO`, communication, ease-of-use, and resonating with the intended target market. The home page should be the most flexible page in terms of customization as it relies heavily on marketing content, branding, and driving diverse messages.

## 6. Design Wire-frames

The wire-frames listed below are meant to represent the app’s home page in various states. These are provided to assist in understanding of what features may look like or their potential use. Final designs may be vastly different from these images.

![Figure 1. Objects can be identified by their shape on the page.](<../../../../.gitbook/assets/APP-FS06-app-home-page-Object Designs.drawio.png>)

![Figure 2. An example home page design.](<../../../../.gitbook/assets/APP-FS06-app-home-page-Home Page.drawio.png>)

{% file src="../../../../.gitbook/assets/APP-FS06-app-home-page.drawio.xml" %}
An editable XML for the app home page (draw.io)
{% endfile %}

## 7. Requirements

Requirements specific to the items listed in this FS are as follows.

### 7.1. Content Sections

The app home page:

**APP-FS06-1:** shall begin with a highlight section, as configured, which may include but is not limited to the following:

* large blocks for featured content
* section for a main message (text header and brief content)
* calls to action with user input controls (buttons)
* carousel sliders for banners
* video displays
* links to other pages

**APP-FS06-2:** shall display, if configured so, any of each basic NFT Store object, which includes but is not limited to:

* Peer Accounts
* Galleries
* Collections
* NFTs
* Transactions

**APP-FS06-3:** shall display, if configured so, content sections which may include but is not limited to the following:

* large blocks for featured content
* section for a secondary message (text header and brief content)
* calls to action with user input controls (buttons)
* carousel sliders for banners
* video displays
* links to other pages

### 7.2. Key Design Characteristics

The app home page, in the context of its key design characteristics:

**APP-FS06-4:** shall incorporate the following key design characteristics:

* Shapes
* Color Gradients
* Layering
* Transparency
* Content Flow
* Interactivity
* Motion

#### 7.2.1. Shapes

Shapes should be used as a signal to the user as to the type of object that they are viewing. See figure 1 in section 6 above for some examples. This is used for universal design as users can immediately identify the object on the screen without a text label. It also helps to separate the content into clearly defined sections. In addition, certain use of shape in the design can create an organic, natural feel which helps users feel at ease with an otherwise overwhelming flood of information. Curves and slightly rounded corners can produce this organic effect.

The app home page:

**APP-FS06-5:** shall make use of a variety of shapes to differentiate object types on the page.

**APP-FS06-6:** shall consistently identify objects by their defined shapes across the app.

Note that identifying shapes may be used in a variety of ways. For example, if an NFT (as an object) is associated with the hexagon shape, it may be shown as an image in a hexagon shaped frame or as a larger square shaped image with a hexagon shaped icon in the upper corner.

**APP-FS06-7:** shall make use of slightly rounded corners on shapes (no sharp corners).

**APP-FS06-8:** shall make use of curved edges, or otherwise organic shapes (blobs, waves, etc.)

#### 7.2.2. Color Gradients

Blending of two or three colors across a block of space, a background, header text, or borders adds to the organic feel of the app.

The app home page:

**APP-FS06-9:** shall make use of color gradients where appropriate.

#### 7.2.3. Layering

Layering shapes can help to show how objects relate to each other. Use of layering also signals to the user the object hierarchy or order of events. Layering also makes the page feel more realistic.

The app home page:

**APP-FS06-10:** shall make use of shape layering to show object relationships, hierarchy, and/or order of events.

**APP-FS06-11:** shall incorporate drop shadows or similar techniques to help illustrate shape layering.

#### 7.2.4. Transparency

The use of transparency when layering objects can produce a modern feel that doesn’t sacrifice the organic feel. Transparency is best used together with blur, parallax, colorization, or a combination of other techniques.

The app home page:

**APP-FS06-12:** shall make use of transparency, with or without related techniques, where appropriate.

#### 7.2.5. Content Flow

The content flow is about providing quick and easy access to what users need, and it’s about delivering a story to the user. The main purpose of the content should be delivered early, the bulk of the content in the middle, and finishing up with detailed info, use-case specific functions, and solutions for edge cases. This can apply to all scales of content across the app (whole pages, user info, NFT info, a pop-up modal, etc.)

The app home page:

**APP-FS06-13:** shall begin with a highlight section.

**APP-FS06-14:** shall introduce new concepts to users from top to bottom, such as the following:

* Galleries are broad in scope and contain collections
* Collections are narrow in scope and contain NFTs
* NFTs, on their own, are atomic (can exist independently of collections or galleries)
* Transactions are happening all the time, are public to some degree, and show what happened between a variety of objects

**APP-FS06-15:** shall allow configuration options to change the manner in which new content is displayed to signal to the user that the new content is something different than the earlier content. For example, one content block may use many small images in a screen width container. The next content block shows a different object. To signal this, the second content block uses larger images in a two column format.

#### 7.2.6. Interactivity

There should be a deep level of interaction between the user and the app. This creates a connection with the user.

The app home page:

**APP-FS06-16:** shall provide an interactive approach to informational displays. For example, hovering the mouse on an NFT image may reveal some quick info about the NFT like its name, price, description, etc.

**APP-FS06-17:** shall incorporate interactivity by providing touch or scroll controls to view more content in content blocks. For example, allow scrolling left and right to see more recent transactions as in figure 2 in section 6 above.

#### 7.2.7. Motion

Motion makes the page feel dynamic. Visually rich applications such as the NFT Store should have motion on the screen (however minimal) at all times, unless the user decides to reduce or disable animations.

The app home page:

**APP-FS06-18:** shall display subtle animations to ensure there’s always some movement on the page. This may be (but not limited to) slowly rotating shapes, waves going across the screen in the background, image borders shining, or color gradients shifting.

## 8. Appendix A: Glossary

| Term   | Meaning                    |
| ------ | -------------------------- |
| RS     | Requirements Specification |
| FS     | Functional Specification   |
| NFT(s) | Non-Fungible Token(s)      |
| UI     | User Interface             |
| UX     | User Experience            |
| SEO    | Search Engine Optimization |
