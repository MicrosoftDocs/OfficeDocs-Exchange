---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
ms.assetid: b828520f-022c-4fcb-ab68-e1c330e87c33
ms.collection:
- Ent_O365_Hybrid
- Strat_EX_EXOBlocker
- exchange-online
- M365-email-calendar
description: 'Summary: Instructions for enabling Exchange Online users to access on-premises public folders in your Exchange Server environment.'
audience: ITPro
f1.keywords:
- NOCSH
title: Configure Exchange Server public folders for a hybrid deployment

---

# Configure Exchange Server public folders for a hybrid deployment

 **Summary**: Instructions for enabling Exchange Online users to access on-premises public folders in your Exchange Server environment.

In a hybrid deployment, your users can be in Exchange Online, on-premises, or both, and your public folders are either in Exchange Online or on-premises. Sometimes your online users may need to access public folders in your Exchange Server on-premises environment. Similarly, Exchange Server users may need to access public folders in Office 365, Microsoft 365, or Exchange Online.

> [!NOTE]
> If you have Exchange 2010 public folders, see [Configure legacy on-premises public folders for a hybrid deployment](set-up-legacy-hybrid-public-folders.md).

This article describes how to enable your Exchange Online, Office 365, and Microsoft 365 users to access public folders in Exchange Server. To enable on-premises Exchange Server users to access public folders in Exchange Online, see [Configure Exchange Online public folders for a hybrid deployment](set-up-exo-hybrid-public-folders.md).

An Exchange Online, Office 365, or Microsoft 365 user must be represented by a MailUser object in the Exchange on-premises environment in order to access Exchange Server public folders. This MailUser object must also be local to the target Exchange Server public folder hierarchy. If you have Exchange Online, Office 365, or Microsoft 365 users who aren't currently represented on-premises by MailUser objects, refer to the Microsoft Knowledge Base article [KB3106618](https://support.microsoft.com/help/3106618) to create matching on-premises entities.

## What do you need to know before you begin?

- These instructions assume that you have used the Hybrid Configuration Wizard to configure and synchronize your on-premises and Exchange Online environments and that the DNS records used for most users' AutoDiscover references an on-premises end-point. For more information, see [Hybrid Configuration Wizard](https://docs.microsoft.com/exchange/hybrid-configuration-wizard).

- Implementing public folder coexistence for a hybrid deployment of Exchange with Office 365 or Microsoft 365 may require you to fix conflicts during the import procedure. Conflicts can happen due to non-routable email addresses assigned to mail enabled public folders, conflicts with other users and groups in Office 365 or Microsoft 365, and other attributes.

- In order to access public folders cross-premises, users must upgrade their Outlook clients to the November 2012 Outlook public update or later.

  - To download the November 2012 Outlook update for Outlook 2010, see [Update for Microsoft Outlook 2010 (KB2687623) 32-Bit Edition](https://www.microsoft.com/download/details.aspx?id=35702).

  - To download the November 2012 Outlook Update for Outlook 2007, see [Update for Microsoft Office Outlook 2007 (KB2687404)](https://www.microsoft.com/download/details.aspx?id=35718).

- Outlook 2016 for Mac, Outlook 2019 for Mac, and Outlook for Mac for Office 365 support cross-premises public folders. If clients in your organization use Outlook 2016 for Mac, make sure they have installed the April 2016 update. Otherwise, those users will not be able to access public folders in a hybrid topology. For more information, see [Accessing public folders with Outlook 2016 for Mac](access-public-folders-with-outlook-2016-for-mac.md).

  > [!NOTE]
  > Outlook 2011 for Mac is not supported for cross-premises public folders. Users must be in the same location as the public folders to access them with Outlook 2011 for Mac. In addition, users whose mailboxes are in Exchange Online won't be able to access on-premises public folders using Outlook on the web (formerly known as Outlook Web App).

- You must synchronize the Active Directory container where your public folder mailboxes are stored (such as the **Users** container) with the AAD Connect tool. Otherwise your public folder mailbox objects won't be synchronized with Exchange Online.

## Step 1: Download the scripts

1. Download the following files from [Mail-enabled Public Folders - directory sync script](https://www.microsoft.com/download/details.aspx?id=46381):

   - `Sync-MailPublicFolders.ps1`

   - `SyncMailPublicFolders.strings.psd1`

2. Save the files to the local computer on which you'll be running PowerShell. For example, C:\PFScripts.

## Step 2: Configure directory synchronization

The Directory Synchronization service doesn't synchronize mail-enabled public folders. Running the following script will synchronize the mail-enabled public folders across premises and Microsoft 365 or Office 365. Special permissions assigned to mail-enabled public folders will need to be recreated in the cloud since cross-premise permission are not supported in Hybrid Deployment scenarios. For more information, see [Exchange Server Hybrid Deployment](https://docs.microsoft.com/exchange/exchange-hybrid).

> [!NOTE]
> Synchronized mail-enabled public folders will appear as mail contact objects for mail flow purposes and will not be viewable in the Exchange admin center. See the Get-MailPublicFolder command. To recreate the SendAs permissions in the cloud, use the Add-RecipientPermission command.

On Exchange Server, run the following command to synchronize mail-enabled public folders from your local on-premises Active Directory to Office 365 or Microsoft 365.

```PowerShell
Sync-MailPublicFolders.ps1 -Credential (Get-Credential) -CsvSummaryFile:sync_summary.csv
```

Where `Credential` is your Microsoft 365 or Office 365 username and password, and `CsvSummaryFile` is the path to where you would like to log synchronization operations and errors, in .csv format.

> [!NOTE]
> Before running the script, we recommend that you first simulate the actions that the script would take in your environment by running it as described above with the `-WhatIf` parameter. > We also recommend that you run this script daily to synchronize your mail-enabled public folders.

## Step 3: Configure Exchange Online users to access Exchange Server on-premises public folders

The final step in this procedure is to configure the Exchange online organization and to allow access to the Exchange Server public folders.

Enable the exchange online organization to access the on-premises public folders. You will point to all of you on-premises public folder mailboxes.

```PowerShell
Set-OrganizationConfig -PublicFoldersEnabled Remote -RemotePublicFolderMailboxes PFMailbox1,PFMailbox2,PFMailbox3
```

> [!NOTE]
> You must wait until ActiveDirectory synchronization has completed to see the changes. This process can take up to 3 hours to complete. If you don't want to wait for the recurring synchronizations that occur every three hours, you can force directory synchronization at any time. For detailed steps to do force directory synchronization, see [Azure AD Connect sync: Scheduler](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler).

## How do I know this worked?

Log on to Outlook for a user who is in Exchange Online and perform the following public folder tests:

- View the hierarchy.

- Check permissions

- Create and delete public folders.

- Post content to and delete content from a public folder.
