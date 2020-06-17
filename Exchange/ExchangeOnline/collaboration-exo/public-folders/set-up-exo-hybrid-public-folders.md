---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
ms.assetid: d979edb3-967b-4431-8beb-0c236bf7f56d
ms.collection:
- Ent_O365_Hybrid
- Strat_EX_EXOBlocker
- exchange-online
- M365-email-calendar
description: 'Summary: Instructions for enabling on-premises Exchange Server users to access public folders in Exchange Online.'
audience: ITPro
f1.keywords:
- NOCSH
title: Configure Exchange Online public folders for a hybrid deployment

---

# Configure Exchange Online public folders for a hybrid deployment

 **Summary**: Instructions for enabling on-premises Exchange Server users to access public folders in Exchange Online.

In a hybrid deployment, your users can be in Exchange Online, on-premises, or both, and your public folders are either in Exchange Online or on-premises. Sometimes your online users may need to access public folders in your Exchange Server on-premises environment. Similarly, Exchange Server users may need to access public folders in Office 365 or Exchange Online.

This article describes how to enable users in your Exchange Server on-premises environment to access Exchange Online, Microsoft 365, or Office 365 public folders. To enable Exchange Online, Microsoft 365, or Office 365 users to access on-premises Exchange Server public folders, see [Configure Exchange Server public folders for a hybrid deployment](set-up-modern-hybrid-public-folders.md).

> [!NOTE]
> If you have Exchange 2010 public folders, see [Configure legacy on-premises public folders for a hybrid deployment](set-up-legacy-hybrid-public-folders.md).

## What do you need to know before you begin?

1. These instructions assume that you have used the Hybrid Configuration Wizard to configure and synchronize your on-premises and Exchange Online environments and that the DNS records used for most users' AutoDiscover references an on-premises end-point. For more information, see [Hybrid Configuration Wizard](https://docs.microsoft.com/exchange/hybrid-configuration-wizard).

2. These instructions assume that Outlook Anywhere is enabled and functional on the on-premises Exchange server(s). For information on how to enable Outlook Anywhere, see [Outlook Anywhere](https://docs.microsoft.com/exchange/outlook-anywhere-exchange-2013-help).

3. Implementing public folder coexistence for a hybrid deployment of Exchange with Office 365 may require you to fix conflicts during the import procedure. Conflicts can happen due to non-routable email address assigned to mail enabled public folders, conflicts with other users and groups in Office 365, and other attributes.

4. In order to access public folders cross-premises, users must upgrade their Outlook clients to the November 2012 Outlook public update or later.

   1. To download the November 2012 Outlook update for Outlook 2010, see [Update for Microsoft Outlook 2010 (KB2687623) 32-Bit Edition](https://www.microsoft.com/download/details.aspx?id=35702).

   2. To download the November 2012 Outlook Update for Outlook 2007, see [Update for Microsoft Office Outlook 2007 (KB2687404)](https://www.catalog.update.microsoft.com/Search.aspx?q=outlook+2007) and download the update in preferred language.

5. Outlook 2011 for Mac and Outlook for Mac for Office 365 are not supported for cross-premises public folders. Users must be in the same location as the public folders to access them with Outlook 2011 for Mac or Outlook for Mac for Office 365. In addition, users whose mailboxes are in Exchange Online won't be able to access on-premises public folders using Outlook on the web (formerly known as Outlook Web App).

   > [!NOTE]
   > Outlook 2016 for Mac is supported for cross-premises public folders. If clients in your organization use Outlook 2016 for Mac, make sure they have installed the April 2016 update. Otherwise, those users will not be able to access public folders in a co-existence or hybrid topology. For more information, see [Accessing public folders with Outlook 2016 for Mac](access-public-folders-with-outlook-2016-for-mac.md).

## Step 1: Download the scripts

1. Download the following files from [Mail-enabled Public Folders - directory sync from EXO to On-prem script](https://go.microsoft.com/fwlink/p/?LinkId=797795).

   - `Import-PublicFolderMailboxes.ps1`

   - `ImportPublicFolderMailboxes.strings.psd1`

   - `Sync-MailPublicFoldersCloudToOnprem.ps1`

   - `Sync-MailPublicFoldersCloudToOnprem.strings.psd1`

2. Save the files to the local computer on which you'll be running PowerShell. For example, C:\PFScripts.

## Step 2: Configure directory synchronization

Running the script `Sync-MailPublicFoldersCloudToOnprem.ps1` will synchronize the mail-enabled public folders between Exchange Online and your Exchange Server on-premises environment. Special permissions assigned to mail-enabled public folders will need to be recreated in the cloud since cross-premise permissions are not supported in Hybrid Deployment scenarios. For more information, see [Exchange Server Hybrid Deployment](https://docs.microsoft.com/exchange/exchange-hybrid).

> [!NOTE]
> Synchronized mail-enabled public folders will appear as mail contact objects for mail flow purposes and will not be viewable in the Exchange admin center. See the Get-MailPublicFolder command. To recreate the SendAs permissions in the cloud, use the Add-RecipientPermission command.

On Exchange Server, run the following command to synchronize mail-enabled public folders from Exchange Online, Microsoft 365, or Office 365 to your local on-premises Active Directory.

```PowerShell
Sync-MailPublicFoldersCloudToOnprem.ps1 -Credential (Get-Credential)
```

Where `Credential` is your Microsoft 365 or Office 365 username and password.

> [!NOTE]
> We recommend that you run this script daily to synchronize your mail-enabled public folders.

## Step 3: Configure on-premises users to access Exchange Online public folders

The final step in this procedure is to configure the Exchange Server on-premises organization to allow access to Exchange Online public folders.

Running the script `Import-PublicFolderMailboxes.ps1` will import public folder mailbox objects from the cloud as mail-enabled users to your on-premises environment. The script will also configure the imported objects as remote public folder mailboxes.

1. On Exchange Server, run the following command to import public folder mailbox objects from the cloud to your on-premises Active Directory.

   ```PowerShell
   Import-PublicFolderMailboxes.ps1 -Credential (Get-Credential)
   ```

   Where `Credential` is your Microsoft 365 or Office 365 username and password.

   > [!NOTE]
   > We recommend that you run this script daily to import your public folder mailbox objects because whenever public folder mailboxes reach their threshold capacity, they automatically split into multiple new mailboxes. Therefore, you always want to ensure you have imported the most recent public folder mailboxes from the cloud.

2. Enable the Exchange 2013 on-premises organization to access the Exchange Online public folders.

   ```PowerShell
   Set-OrganizationConfig -PublicFoldersEnabled Remote
   ```

   > [!NOTE]
   > You must wait until ActiveDirectory synchronization has completed to see the changes. This process can take up to 30 minutes to complete. If you don't want to wait for the recurring synchronizations that occur every 30 minutes, you can force directory synchronization at any time. For detailed steps to do force directory synchronization, see [Azure AD Connect sync: Scheduler](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler).

## How do I know this worked?

Log on to Outlook for a user who is in Exchange Online and perform the following public folder tests:

- View the hierarchy.

- Check permissions

- Create and delete public folders.

- Post content to and delete content from a public folder.
