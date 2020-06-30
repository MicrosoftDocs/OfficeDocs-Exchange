---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.assetid: bcb0ac98-2949-486b-a8ab-8549c021651b
ms.reviewer:
ms.collection:
- Ent_O365_Hybrid
- Strat_EX_EXOBlocker
- exchange-server
description: 'Summary: Use the steps in this article to synchronize public folders between Exchange Online and your on-premises Exchange deployment.'
f1.keywords:
- NOCSH
audience: ITPro
title: Configure legacy on-premises public folders for a hybrid deployment

---

# Configure legacy on-premises public folders for a hybrid deployment

In a hybrid deployment, your users can be in Exchange Online, Exchange on-premises, or both, and your public folders are either in Exchange Online or Exchange on-premises. Public folders can only reside in one place, so you must decide whether your public folders will be in Exchange Online or on-premises. They can't be in both locations. Public folder mailboxes are synchronized to Exchange Online by the Directory Synchronization service. However, mail-enabled public folders aren't synchronized across premises.

This article describes how to synchronize mail-enabled public folders when your users are in Microsoft 365 or Office 365 and your Exchange 2010 SP3 or later version public folders are on-premises. However, a Microsoft 365 or Office 365 user who is not represented by a MailUser object on-premises (local to the target public folder hierarchy) won't be able to access legacy or Exchange on-premises public folders.

> [!NOTE]
> This topic refers to the Exchange 2010 SP3 or later servers as the *legacy Exchange server*.

You will use the following scripts to sync your mail-enabled public folders. The scripts are initiated by a Windows task that runs in the on-premises environment:

- `Sync-MailPublicFolders.ps1`: This script synchronizes mail-enabled public folder objects from your local Exchange on-premises deployment with Microsoft 365 or Office 365. It uses the local Exchange on-premises deployment as master to determine what changes need to be applied to Microsoft 365 or Office 365. The script will create, update, or delete mail-enabled public folder objects on Microsoft 365 or Office 365 Active Directory based on what exists in the local on-premises Exchange deployment.

- `SyncMailPublicFolders.strings.psd1`: This is a support file used by the preceding synchronization script and should be copied to the same location as the preceding script.

When you complete this procedure your on-premises and Microsoft 365 or Office 365 users will be able to access the same on-premises public folder infrastructure.

## What hybrid versions of Exchange will work with public folders?

The following table describes the supported version and location combinations of user mailboxes and public folders. "Hybrid not applicable" is still a supported scenario, but is not considered a hybrid scenario because both the public folders and the users are residing in the same location.

|**Scenario**|**On-Premises Exchange 2010 User Mailbox**|**On-Premises Exchange 2016/2019 User Mailbox**|**Exchange Online User Mailbox**|
|:-----|:-----|:-----|:-----|
|On-Premises Exchange 2010 Public Folders|Hybrid not applicable|Hybrid not applicable|Supported|
|On-Premises Exchange 2013, Exchange 2016, or Exchange 2019 Public Folders|Hybrid not applicable|Hybrid not applicable|Supported|
|Exchange Online Public Folders|Not supported|Supported|Hybrid not applicable|

A hybrid configuration with Exchange 2003 public folders is not supported. If you're running Exchange 2003 in your organization, you must move all public folder databases and replicas to Exchange 2010 SP3 or later. No public folder replicas can remain on Exchange 2003.

## Step 1: What do you need to know before you begin?

- These instructions assume that you have used the Hybrid Configuration Wizard to configure and synchronize your on-premises and Exchange Online environments and that the DNS records used for most users' Autodiscover references an on-premises end-point. For more information, see [Hybrid Configuration Wizard](https://docs.microsoft.com/exchange/hybrid-configuration-wizard).

- These instructions assume that Outlook Anywhere is enabled and functional on the on-premises legacy Exchange servers. For information on how to enable Outlook Anywhere, see [Outlook Anywhere](https://docs.microsoft.com/exchange/outlook-anywhere-exchange-2013-help).

- Implementing legacy public folder coexistence for a hybrid deployment of Exchange with Microsoft 365 or Office 365 may require you to fix conflicts during the import procedure. Conflicts can happen due to non-routable email address assigned to mail enabled public folders, conflicts with other users and groups in Microsoft 365 or Office 365, and other attributes.

- These instructions assume your Exchange Online organization has been upgraded to a version that supports public folders.

- In Exchange Online, you must be a member of the Organization Management role group. This role group is different from the permissions assigned to you when you subscribe to Exchange Online. For details about how to enable the Organization Management role group, see [Manage role groups](../../permissions/role-groups.md).

- In Exchange 2010, you must be a member of the Organization Management or Server Management Role Based Access Control (RBAC) role groups. For details, see [Add Members to a Role Group](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/dd638143(v=exchg.141)).

- In order to access public folders cross-premises, users must upgrade their Outlook clients to the November 2012 or later Outlook public update.

  - To download the November 2012 Outlook update for Outlook 2010, see [Update for Microsoft Outlook 2010 (KB2687623) 32-Bit Edition](https://www.microsoft.com/download/details.aspx?id=35702).

  - To download the November 2012 Outlook update for Outlook 2007, see [Update for Microsoft Office Outlook 2007 (KB2687404)](https://www.catalog.update.microsoft.com/Search.aspx?q=outlook+2007) and download in preferred language.

- Outlook 2016 for Mac and Outlook for Mac for Microsoft 365 or Office 365 are supported for cross-premises public folders if the following conditions are true:

  - The April 2016 update for Outlook 2016 for Mac is installed.

  - Exchange 2016 CU2 or later.

  - Exchange 2013 CU14 or later.

- After you have followed the instructions in this article to configure your on-premises public folders for a hybrid deployment, users who are external to your organization won't be able to send messages to your on-premises public folders unless you take additional steps. You can either set the accepted domain for the public folders to Internal Relay (see [Manage accepted domains in Exchange Online](https://docs.microsoft.com/exchange/mail-flow-best-practices/manage-accepted-domains/manage-accepted-domains) for more information) or you can disable Directory Based Edge Blocking (DBEB), as described in [Use Directory Based Edge Blocking to Reject Messages Sent to Invalid Recipients](https://docs.microsoft.com/exchange/mail-flow-best-practices/use-directory-based-edge-blocking).

- In hybrid mode, Exchange Online users can't access public folders using Outlook on the web (formerly known as Outlook Web App).

## Step 2: Make remote public folders discoverable

1. If your public folders are on Exchange 2010 servers, you must install Client Access services on all mailbox servers that have a public folder database. This enables the Exchange RpcClientAccess service to run, which enables all clients to access public folders. For more information, see [Install Exchange Server 2010](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124778(v=exchg.141)).

   > [!NOTE]
   > This server doesn't have to be part of the Client Access load balancing. For more information, see [Understanding Load Balancing in Exchange 2010](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ff625247(v=exchg.141)).

2. Create an empty mailbox database on each public folder server.

   For Exchange 2010, run the following command in the Exchange Management Shell. This command excludes the mailbox database from the mailbox provisioning load balancer. This prevents new mailboxes from automatically being added to this database.

   ```PowerShell
   New-MailboxDatabase -Server <PFServerName_with_CASRole> -Name <NewMDBforPFs> -IsExcludedFromProvisioning $true
   ```

   For Exchange 2007, run the following command in the Exchange Management Shell:

   ```PowerShell
   New-MailboxDatabase -StorageGroup "<PFServerName>\StorageGroup>" -Name <NewMDBforPFs>
   ```

   > [!NOTE]
   > We recommend that the only mailbox that you add to this database is the proxy mailbox that you'll create in the next step. No other mailboxes should be created on this mailbox database.

3. Create a proxy mailbox within the new mailbox database, and hide the mailbox from the address book. The SMTP of this mailbox will be returned by AutoDiscover as the _DefaultPublicFolderMailbox_ SMTP, so that by resolving this SMTP the client can reach the legacy exchange server for public folder access.

   ```PowerShell
   New-Mailbox -Name <PFMailbox1> -Database <NewMDBforPFs>
   ```

   ```PowerShell
   Set-Mailbox -Identity <PFMailbox1> -HiddenFromAddressListsEnabled $true
   ```

4. For Exchange 2010, enable Autodiscover to return the proxy public folder mailboxes.

   ```PowerShell
   Set-MailboxDatabase <NewMDBforPFs> -RPCClientAccessServer <PFServerName_with_CASRole>
   ```

5. Repeat the preceding steps for every public folder server in your organization.

## Step 3: Download the scripts

1. Download the following files from [Mail-enabled Public Folders - directory sync script](https://www.microsoft.com/download/details.aspx?id=46381):

   - `Sync-MailPublicFolders.ps1`

   - `SyncMailPublicFolders.strings.psd1`

2. Save the files to the local computer on which you'll be running PowerShell. For example, C:\PFScripts.

## Step 4: Configure directory synchronization

The Directory Synchronization service doesn't synchronize mail-enabled public folders. Running the following script will synchronize the mail-enabled public folders across premises. Special permissions assigned to mail-enabled public folders will need to be recreated in the cloud since cross-premise permission are not supported in Hybrid Deployment scenarios.

> [!NOTE]
> Synchronized mail-enabled public folders will appear as mail contact objects for mail flow purposes and will not be viewable in the Exchange admin center. See the Get-MailPublicFolder command. To recreate the SendAs permissions in the cloud, use the Add-RecipientPermission command.

1. On the legacy Exchange server, run the following command to synchronize mail-enabled public folders from your local on-premises Active Directory to Microsoft 365 or Office 365.

   ```PowerShell
   Sync-MailPublicFolders.ps1 -Credential (Get-Credential) -CsvSummaryFile:sync_summary.csv
   ```

   Where `Credential` is your Microsoft 365 or Office 365 name and password, and `CsvSummaryFile` is the path to where you would like to log synchronization operations and errors, in .csv format.

> [!NOTE]
> Before running the script, we recommend that you first simulate the actions that the script would take in your environment by running it as described above with the `-WhatIf` parameter. We also recommend that you run this script daily to synchronize your mail-enabled public folders.

## Step 5: Configure Exchange Online users to access on-premises public folders

The final step in this procedure is to configure the Exchange Online organization and to allow access to the legacy on-premises public folders.

You will point to all of the proxy public folder mailboxes that you created in [Step 2: Make remote public folders discoverable](#step-2-make-remote-public-folders-discoverable) to enable theExchange Online organization to access the on-premises public folders.

Run the following command in Exchange Online PowerShell. To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

```PowerShell
Set-OrganizationConfig -PublicFoldersEnabled Remote -RemotePublicFolderMailboxes PFMailbox1,PFMailbox2,PFMailbox3
```

You must wait until Active Directory synchronization has completed to see the changes. This process can take up to 3 hours to complete. If you don't want to wait for the recurring synchronizations that occur every three hours, you can force directory synchronization at any time. For detailed steps to force directory synchronization, see [Azure AD Connect sync: Scheduler](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler). Microsoft 365 or Office 365 randomly selects one of the public folder mailboxes that's supplied in this command.

> [!IMPORTANT]
> A Microsoft 365 or Office 365 user who is not represented by a MailUser object on-premises (local to the target public folder hierarchy) won't be able to access legacy, Exchange 2016, or Exchange 2019 on-premises public folders. See the Knowledge Base article [Exchange Online users can't access legacy on-premises public folders](https://support.microsoft.com/help/3106618) for a solution.

## How do I know this worked?

Log on to Outlook for a user who is in Exchange Online and perform the following public folder tests:

- View the hierarchy.

- Check permissions

- Create and delete public folders.

- Post content to and delete content from a public folder.
