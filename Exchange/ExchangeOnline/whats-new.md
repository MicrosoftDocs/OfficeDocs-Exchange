---
localization_priority: Normal
description: What's new in Exchange admin center
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid:
ms.reviewer: 
title: What's new in Exchange admin center
f1.keywords:
- NOCSH
ms.collection: 
- exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars
---

# What's new in Exchange admin center

We're continuously adding new features to Exchange admin center (EAC), fixing issues as we learn about them, and making changes based on your feedback. On this page, you can find highlights of all the recent changes we've made. Some features get rolled out at different times to our customers, so if you are not seeing a new feature yet, keep checking back.

Exchange admin center now uses a new portal at [https://admin.exchange.microsoft.com](https://admin.exchange.microsoft.com). This is a modern, web-based management console for managing Exchange, designed to provide an experience that is more aligned with the overall M365 admin experience.

For now, it is possible to switch back to the existing EAC (often called the "classic" EAC), but at a future date, the classic EAC will be retired.

## April 2020

Here are some of the changes and new features we introduced in the modern EAC in April 2020.

### Contacts

Admins now have a new experience when managing contacts for people outside the organization. Admins can create and manage mail contacts and mail users with external email addresses.

![Screen capture of contacts](./media/whats-new-2020-04--1-contacts.png)

### Column chooser

Admins can now customize the columns that appear in the EAC.

![Screen capture of column chooser](./media/whats-new-2020-04--2-column-chooser.png)

### People picker for remote migration

A very common request from our customers was to bring back the people picker for a remote migration scenario. This helps admins to move the selected mailboxes to Exchange online.

![Screen capture of people picker for remote migration](./media/whats-new-2020-04--3-people-picker.png)

### Personalized Dashboard and Reports

Exchange admins can now use a dashboard to choose from a wide variety of cards that personalize their experience for ease of use and better productivity. To access the dashboard, go to the Exchange admin center and select **Add Card (+)** to see the new cards:

- **Migration report**: Learn about the status of the migration batches in your Exchange environment.
- **Mail flow reports**: Discover and understand trends related to mail flow in your Office 365 organization. These reports were already available in the Security & Compliance Center (SCC) portal, but are now available in the EAC for added convenience.
  - **Auto-forwarded messages**: Monitor for potential data leaks when people in your organization automatically forward email messages to an external domain, such as a personal email address. [Learn more](https://docs.microsoft.com/microsoft-365/security/office-365-security/mfi-auto-forwarded-messages-report).
  - **Inbound & outbound messages details**: Monitor message volume and TLS encryption for each connector. [Learn more](https://docs.microsoft.com/microsoft-365/security/office-365-security/mfi-outbound-and-inbound-mail-flow).
  - **Non-accepted domain**: Display messages from your on-premises organization where the sender's email domain isn't configured as an accepted domain in Office 365. [Learn more](https://docs.microsoft.com/office365/securitycompliance/mfi-non-accepted-domain-report).
  - **Non-delivery report**: Display the most commonly encountered error codes in non-delivery reports (also known as NDRs or bounce messages) for your message senders. [Learn more](https://docs.microsoft.com/office365/securitycompliance/mfi-non-delivery-report).

![Screen capture of dashboard](./media/whats-new-2020-04--4-dashboard.png)

### Recoverable Items

Admins now have a new experience for finding recoverable items. With this feature, items that were deleted from a user's mailbox can be recovered back to the inbox.

![Screen capture of what's new in dashboard](./media/whats-new-2020-04--5-recoverable-items.png)

## March 2020

Here are some of the changes and new features we introduced in the modern EAC in March 2020.

#### Recipients

In the modern EAC, the user and shared mailbox management experiences are now merged, and the mailbox list and properties are available on the same page. The option to filter mailboxes based on type can be found on the top right.

Resources experience has been simplified for managing room and resource mailboxes.

![Screen capture of what's new in recipients](./media/whats-new-2020-03-recipients.png)

#### Migration

Migration is now a first-class citizen under the Modern EAC and is no longer buried inside the Recipient tab as it was in the classic EAC. The major feature additions to the migration flow are:

  - The Exchange and G-Suite migrations are now simple, wizard-based experiences.
  - The G-Suite migration supports migrating Calendar and Contact data along with emails.
  - For G-Suite migration, the 2GB per mailbox per day restriction has been removed.

![Screen capture of what's new in migration](./media/whats-new-2020-03-migration.png)

#### Mail flow  

The Mail flow Experience, which was a part of the Security and compliance portal, is now returning to EAC. As a part of the experience, we have added the following features:

  - Accepted domains
  - Remote domains
  - Connectors

![Screen capture of what's new in mail flow](./media/whats-new-2020-03-mailflow.png)

#### Want to access more features?

As the modern experience is being developed, we are providing deep links from the new portal for users to move to the old portal for completing their work.

To access familiar features that were in the classic Exchange admin center, click on the "More features" tab on the left nav and select the feature to open it in a new tab.

![Screen capture of more features](./media/whats-new-2020-03-morefeatures.png)

## What's next?

We are working hard to create modern experiences for Exchange admins. Here are some features that are coming soon:

  - Parity Experience with the classic EAC
      - Groups
      - Permissions
      - Organization
      - Public Folders
  - New Value additions for customers
      - Recommendations
      - Search
      - G-Suite Automation

Check out our [Ignite blog entry](https://techcommunity.microsoft.com/t5/exchange-team-blog/exchange-admin-improvements-announced-at-microsoft-ignite-2019/ba-p/982121) where we detail the changes to the Exchange admin center, as well as other Exchange Online improvements that we announced at Microsoft Ignite 2019.

## Feedback and wishlist

Our goal is to deliver the features that IT admins need, so please share your feedback and wishlist with us through the "Give Feedback" button on the new portal.
