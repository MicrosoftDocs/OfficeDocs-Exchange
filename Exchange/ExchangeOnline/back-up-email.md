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
> You can get the latest information related to a service interrupting event by logging into the Service Health Dashboard. For more information, see [View the status of your services](https://go.microsoft.com/fwlink/p/?LinkId=786661).

## What happens if users accidentally delete data from their mailboxes?

Deleted items are stored in the Deleted Items folder of the mailbox. Items removed from the Deleted Items folder or deleted by pressing Shift+Delete are most likely recoverable if they're dealt with promptly.

For more information about how admins can recover deleted items in Exchange Online, see the following topics:

- [Recoverable Items folder in Exchange Online](security-and-compliance/recoverable-items-folder/recoverable-items-folder.md).

- [Enable or disable single item recovery for a mailbox in Exchange Online](recipients-in-exchange-online/manage-user-mailboxes/enable-or-disable-single-item-recovery.md)

- [Change how long permanently deleted items are kept for an Exchange Online mailbox](recipients-in-exchange-online/manage-user-mailboxes/change-deleted-item-retention.md).

> [!NOTE]
> Point in time restoration of mailbox items is out of scope for the Exchange Online service (there might be 3rd party solutions providing that functionality). However, Exchange Online offers great retention and recovery support for your organization's email infrastructure, and your mailbox data is available when you need it, no matter what happens. For more information about additional options, see the following topics:
>
> - [High Availability and Business Continuity](https://technet.microsoft.com/library/7b03465e-3b9c-4500-8956-a83377f4c2c3.aspx)
> - [Exchange Online Service Description](https://technet.microsoft.com/library/7a83da3c-3b6d-4f86-ad4d-6104707cd0ec.aspx)
> - [In-Place Hold and Litigation Hold](security-and-compliance/in-place-and-litigation-holds.md)
> - [Office 365 retention policies](https://docs.microsoft.com/office365/securitycompliance/retention-policies)
> - [Inactive mailboxes in Office 365](https://docs.microsoft.com/office365/securitycompliance/inactive-mailboxes-in-office-365)

## How do users backup Outlook data?

Exchange Online does not provide a way to perform a traditional backup of mailboxes (as this implies the ability to restore a mailbox to a state the mailbox was in when the backup was taken). The best way to provide additional storage for user emails is with Exchange Online Archiving. Using Outlook to backup data into PST files aren't recommended due to the loss of discoverability and control of the content.

For more information about Exchange Online Archiving, see:

- [Enable archive mailboxes in the Office 365 Security & Compliance Center](https://docs.microsoft.com/office365/securitycompliance/enable-archive-mailboxes)

- [Unlimited archiving in Office 365](https://docs.microsoft.com/office365/securitycompliance/unlimited-archiving)

For more information about the licensing requirements for Exchange Online Archiving, see the [Exchange Online Archiving Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-archiving-service-description/exchange-online-archiving-service-description).


## How your data is protected

To learn how the service is protected using Data Resiliency, see [Office 365 Exchange Data Resiliency](https://docs.microsoft.com/Office365/securitycompliance/office-365-exchange-data-resiliency).

## When Can I Restore Outlook data on an Office 365 account without a license?

After the expiration or removal of an O365 License, Office 365 doesn't **instantly** remove your data. The default Office 365 retention time is 30 days; this means that you can renew or backup your data into PST before the data is entirely removed from O365.

## How do users restore Outlook data?

To learn how to restore deleted items in Outlook, see [Recover deleted items in Outlook](https://support.office.com/article/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce).

To learn how to restore deleted items in Outlook on the web (formerly known as Outlook Web App), see [Recover deleted items or email in Outlook on the web](https://support.office.com/article/c3d8fc15-eeef-4f1c-81df-e27964b7edd4).

## Offboard a user from Office 365

For more info what to do when a user in your organization leaves, check out [Remove a former employee from Office 365](https://go.microsoft.com/fwlink/p/?LinkId=816871). This topic discusses the steps you should take and how to secure your data after an employee leaves your organization.
