---
title: 'Shared mailboxes: Exchange 2013 Help'
TOCTitle: Shared mailboxes
ms:assetid: 1d71c01b-e261-408e-a633-1d1c9d00032a
ms:mtpsurl: https://technet.microsoft.com/library/JJ150498(v=EXCHG.150)
ms:contentKeyID: 47559964
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Shared mailboxes

_**Applies to:** Exchange Server 2013_

Learn about the Exchange shared mailbox in Microsoft Exchange Server 2013, the reasons to use them, and how to convert a delegated mailbox to an Exchange shared mailbox.

A shared mailbox is a mailbox that multiple users can use to read and send email messages. Shared mailboxes can also be used to provide a common calendar, allowing multiple users to schedule and view vacation time or work shifts.

Shared mailboxes aren't supported on mobile devices.

**Why set up a shared mailbox?**

  - Provides a generic email address (for example, info@contoso.com or sales@contoso.com), that customers can use to inquire about your company.

  - Allows departments that provide centralized services to employees (for example, help desk, human resources, or printing services), to respond to employee questions.

  - Allows multiple users to monitor and reply to email sent to an email address (for example, an address used specifically by the help desk).

## What are shared mailboxes?

A shared mailbox is a type of user mailbox that doesn't have its own username and password. As a result, users can't log into it them directly. To access a shared mailbox, users must first be granted Send As or Full Access permissions to the mailbox. Once that's done, users sign into their own mailboxes and then access the shared mailbox by adding it to their Outlook profile. In Exchange 2003 and earlier, shared mailboxes were just a regular mailbox to which an administrator could grant delegate access. Beginning in Exchange 2007, shared mailboxes became their own recipient type:

  - **RecipientType:** UserMailbox

  - **RecipientTypeDetails:** SharedMailbox

In previous version of Exchange, creating a shared mailbox was a multi-step process in which you had to use the Exchange Management Shell to complete some of the tasks. In Exchange 2013, you can use the Exchange admin center (EAC) to create a shared mailbox in one step. For details, see [Create a shared mailbox](create-a-shared-mailbox-exchange-2013-help.md). In fact, the EAC has a feature area devoted entirely to shared mailboxes. Just navigate to **Recipients** \> **Shared mailboxes** to view all the management tasks for shared mailboxes.

You can use the following permissions with a shared mailbox.

  - **Full Access**: The Full Access permission lets a user open the shared mailbox and act as the owner of that mailbox. After accessing the shared mailbox, a user can create calendar items; read, view, delete, and change email messages; create tasks and calendar contacts. However, a user with Full Access permission can't send email from the shared mailbox unless they also have Send As or Send on Behalf permission.

  - **Send As**: The Send As permission lets a user impersonate the shared mailbox when sending mail. For example, if Kweku logs into the shared mailbox Marketing Department and sends an email, it will look like the Marketing Department sent the email.

  - **Send on Behalf**: The Send on Behalf permission lets a user send email on behalf of the shared mailbox. For example, if John logs into the shared mailbox Reception Building 32 and sends an email, it look like the mail was sent by "John on behalf of Reception Building 32". You can't use the EAC to grant Send on Behalf permissions, you must use [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox) cmdlet with the *GrantSendonBehalf* parameter.

## Converting shared mailboxes

In previous versions of Exchange, you could use a regular mailbox as a delegated mailbox. If you have delegated mailboxes, you can use the Shell to convert those delegate mailboxes to shared mailboxes. For details, see [Convert a Mailbox](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-user-mailboxes/convert-a-mailbox).
