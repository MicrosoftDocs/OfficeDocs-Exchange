---
localization_priority: Normal
description: Admins can learn about the deleted message recovery options and high-level methods that Exchange Online uses to protect mailbox data.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 94d2f62e-5d43-4200-b7ce-33b1f41f1d59
ms.reviewer: 
title: Back up email in Exchange Online
f1.keywords:
- NOCSH
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Backing up email in Exchange Online

One of the questions we often hear is "How does Exchange Online back up my data?" You may be asking this because you're concerned about how to recover your data if there is a failure. Alternatively, you may be wondering how to recover your data if it gets accidentally deleted. This topic answers these questions.

## How does Exchange Online protect mailbox data?

Lots of things can disrupt service availability, such as hardware failure, natural disasters, or human error. To ensure that your data is always available and that services continue, even when unexpected events occur, Exchange Online uses the same technologies found in Exchange Server. For example, Exchange Online uses the Exchange Server feature known as Database Availability Groups (DAGs) to replicate Exchange Online mailboxes to multiple databases in separate Microsoft datacenters. 

As a result, you can readily access up-to-date mailbox data in the event of a failure that affects one of the database copies. In addition to having multiple copies of each mailbox database, the different datacenters enforce the data using replication (data resiliency). If one fails, the affected data are transferred to another data center with limited service interruption, and users experience seamless connectivity.


> [!NOTE]
> You can get the latest information related to a service interrupting event by logging into the Service Health Dashboard. For more information, see [How to check Microsoft 365 service health](https://docs.microsoft.com/office365/enterprise/view-service-health).

## What happens if users accidentally delete data from their mailboxes?

Deleted items are stored in the Deleted Items folder of the mailbox. Items removed from the Deleted Items folder or deleted by pressing Shift+Delete are most likely recoverable if they're dealt with promptly.

For more information about how admins can recover deleted items in Exchange Online, see the following topics:

- [Recoverable Items folder in Exchange Online](security-and-compliance/recoverable-items-folder/recoverable-items-folder.md).

- [Enable or disable single item recovery for a mailbox in Exchange Online](recipients-in-exchange-online/manage-user-mailboxes/enable-or-disable-single-item-recovery.md)

- [Change how long permanently deleted items are kept for an Exchange Online mailbox](recipients-in-exchange-online/manage-user-mailboxes/change-deleted-item-retention.md).

> [!NOTE]
> Point in time restoration of mailbox items is out of scope for the Exchange Online service, though there might be third-party solutions available that provide this functionality. Exchange Online offers great retention and recovery support for your organization's email infrastructure, and your mailbox data is available when you need it, no matter what happens. For more information about additional options, see the following topics:
>
> - [High availability and business continuity](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/high-availability-and-business-continuity)
> - [Exchange Online service description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description)
> - [In-place hold and litigation hold](security-and-compliance/in-place-and-litigation-holds.md)
> - [Retention policies](https://docs.microsoft.com/office365/securitycompliance/retention-policies)
> - [Inactive mailboxes](https://docs.microsoft.com/office365/securitycompliance/inactive-mailboxes-in-office-365)

## How do users backup Outlook data?

Exchange Online does not provide a way to perform a traditional backup of mailboxes. That is, there is no way to restore a mailbox to the state the mailbox was in when the backup was taken.

However, if you need to provide additional storage for user emails, the best way is to use Exchange Online Archiving. Using Outlook to backup data into PST files isn't recommended due to the loss of discoverability and control of the content.

For more information about Exchange Online Archiving, see:

- [Enable archive mailboxes in the Microsoft 365 security and compliance centers](https://docs.microsoft.com/office365/securitycompliance/enable-archive-mailboxes)

- [Unlimited archiving in Office 365](https://docs.microsoft.com/office365/securitycompliance/unlimited-archiving)

For more information about the licensing requirements for Exchange Online Archiving, see the [Exchange Online Archiving service description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-archiving-service-description/exchange-online-archiving-service-description).

## How your data is protected

To learn how the service is protected using Data Resiliency, see [Exchange Online Data Resiliency in Office 365](https://docs.microsoft.com/Office365/securitycompliance/office-365-exchange-data-resiliency).

## When Can I Restore Outlook data on a Microsoft 365 or Office 365 account without a license?

After the expiration or removal of a Microsoft 365 or Office 365 license, your data is not **instantly** removed. The default retention time is 30 days; this means that you can renew or backup your data into PST before the data is entirely removed from Microsoft 365 or Office 365.

## How do users restore Outlook data?

To learn how to restore deleted items in Outlook, see [Recover deleted items in Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce).

To learn how to restore deleted items in Outlook on the web (formerly known as Outlook Web App), see [Recover deleted items or email in Outlook on the web](https://support.microsoft.com/office/c3d8fc15-eeef-4f1c-81df-e27964b7edd4).

## Offboard a user from Microsoft 365 or Office 365

For more info what to do when a user in your organization leaves, check out [Remove a former employee](https://docs.microsoft.com/microsoft-365/admin/add-users/remove-former-employee). This topic discusses the steps you should take and how to secure your data after an employee leaves your organization.
