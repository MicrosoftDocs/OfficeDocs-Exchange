---
localization_priority: Normal
description: LinkedIn contact synchronization lets people set up a connection between their LinkedIn account and their Microsoft 365 or Office 365 account by using Outlook on the web. After they set up LinkedIn contact sync, all their LinkedIn connections are listed as contacts in People in Microsoft 365 or Office 365. They can then interact with their LinkedIn connections as they do with other contacts. LinkedIn contact sync is turned on by default if the feature is available for your region.
ms.topic: article
author: mattpennathe3rd
ms.custom:
- ms.exch.eac.EditOwaMailboxPolicy_LinkedIn
ms.author: v-mapenn
ms.assetid: 54d9b046-4f36-4f36-b88a-95b1da4e1b4c
ms.reviewer: 
f1.keywords:
- CSH
title: Manage LinkedIn contact sync in your organization
ms.collection: 
- exchange-online
- M365-email-calendar
audience: Developer
ms.service: exchange-online
manager: serdars

---

# Manage LinkedIn contact sync in your organization

LinkedIn contact synchronization lets people set up a connection between their LinkedIn account and their Microsoft 365 or Office 365 account by using Outlook on the web (formerly known as Outlook Web App). After they set up LinkedIn contact sync, all their LinkedIn connections are listed as contacts in People in Microsoft 365 or Office 365. They can then interact with their LinkedIn connections as they do with other contacts. LinkedIn contact sync is turned on by default if the feature is available for your region.

> [!TIP]
> 
> As an admin, you probably want to keep LinkedIn contact sync turned on if your organization uses LinkedIn for business purposes, such as networking and marketing. Turn it off if you don't want your users to download their LinkedIn connections as contacts in Outlook on the web.
> 
> The features that are available to your Microsoft 365 or Office 365 organization are determined by the service plan for your account. Some features aren't available to mailboxes or organizations in specific regions.

## Turn LinkedIn contact sync on or off

You turn LinkedIn contact sync on or off for users in your organization by using Outlook on the web mailbox policy (formerly known as Outlook Web App mailbox policy) settings. Similar to other Outlook on the web mailbox policy settings, you can change the settings for LinkedIn contact sync by using the Exchange admin center (EAC) or Exchange Online PowerShell. For detailed information about managing Outlook on the web mailbox policy settings, see [View or configure Outlook on the web mailbox policy properties](../clients-and-mobile-in-exchange-online/outlook-on-the-web/configure-outlook-web-app-mailbox-policy-properties.md).

## For more information

The information for each LinkedIn contact is stored as a read-only contact record in the LinkedIn folder in People. The information that's synchronized between LinkedIn and Outlook on the web includes first name, last name, all phone numbers, all email addresses, and all street addresses. LinkedIn contacts are stored in the user's mailbox and are retained in accordance with the Microsoft 365 or Office 365 service plan. For information about how your users can set up a connection to LinkedIn using a desktop version of Outlook, have them check out [Social Connector for Microsoft Outlook](https://support.microsoft.com/office/255447e8-82cd-48e7-9b79-1dd8721a2907).
