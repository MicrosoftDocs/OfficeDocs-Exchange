---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
ms.assetid: bcb0ac98-2949-486b-a8ab-8549c021651b
ms.collection:
- Strat_EX_EXOBlocker
- Ent_O365_Hybrid
- exchange-online
- M365-email-calendar
description: 'Summary: Use the steps in this article to synchronize public folders between Microsoft 365 or Office 365 and your Exchange Server 2010 on-premises deployment.'
audience: ITPro
f1.keywords:
- NOCSH
title: Configure legacy on-premises public folders for a hybrid deployment

---

# Configure legacy on-premises public folders for a hybrid deployment

 **Summary**: Use the steps in this article to synchronize public folders between Microsoft 365 or Office 365 and your Exchange Server 2010 on-premises deployment.

In a hybrid deployment, your users can be in Exchange Online , on-premises, or both, and your public folders are either in Exchange Online or on-premises. Public folders can reside in only one place, so you must decide whether your public folders will be in Exchange Online or on-premises. They can't be in both locations. Public folder mailboxes are synchronized to Exchange Online by the Directory Synchronization service. However, mail-enabled public folders aren't synchronized across premises.

This topic describes how to synchronize mail-enabled public folders if your users are in Microsoft 365 or Office 365 and your Exchange Server 2010 SP3 public folders are on-premises. However, a Microsoft 365 or Office 365 user who is not represented by a MailUser object on-premises (local to the target public folder hierarchy) won't be able to access legacy or modern on-premises public folders.

> [!NOTE]
> This topic refers to the Exchange Server 2010 SP3 servers as the legacy Exchange server.

You will sync your mail-enabled public folders by using the following scripts, which are initiated by a Windows task that runs in the on-premises environment:

- `Sync-MailPublicFolders.ps1`: This script synchronizes mail-enabled public folder objects from your local Exchange on-premises deployment with Microsoft 365 or Office 365. It uses the local Exchange on-premises deployment as master to determine what changes need to be applied to Microsoft 365 or Office 365. The script will create, update, or delete mail-enabled public folder objects on Microsoft 365 or Office 365 Active Directory based on what exists in the local on-premises Exchange deployment.

- `SyncMailPublicFolders.strings.psd1`: This is a support file used by the preceding synchronization script and should be copied to the same location as the preceding script.

When you complete this procedure your on-premises and Microsoft 365 or Office 365 users will be able to access the same on-premises public folder infrastructure.

## What hybrid versions of Exchange will work with public folders?

The following table describes the version and location combinations of user mailboxes and public folders that are supported. "Hybrid not applicable" is still a supported scenario, but is not considered a hybrid scenario since both the public folders and the users are residing in the same location.

||**On-Premises Exchange 2010 User Mailbox**|**On-Premises Exchange 2013 User Mailbox**|**Exchange Online User Mailbox**|
|:-----|:-----|:-----|:-----|
|On-Premises Exchange 2010 Public Folders|Hybrid not applicable|Hybrid not applicable|Supported|
|On-Premises Exchange 2013 Public Folders|Hybrid not applicable|Hybrid not applicable|Supported|
|Exchange Online Public Folders|Not supported|Supported|Hybrid not applicable|

> [!NOTE]
> Outlook 2016 does not support accessing Exchange 2007 legacy public folders. If you have users who are using Outlook 2016, you must move your public folders to a more recent version of Exchange Server. More information about Outlook 2016 and Office 2016 compatibility with Exchange 2007 and earlier versions can be found in [this article](https://support.microsoft.com/office/2ab9e8ef-4cd9-4041-9426-73e8f6c5aacc).

## Step 1: What do you have to know before you begin?

- These instructions assume that you have used the Hybrid Configuration Wizard to configure and synchronize your on-premises and Exchange Online environments, and that the DNS records that are used for the Autodiscover service for most users reference an on-premises end point. For more information, see [Hybrid Configuration Wizard](https://docs.microsoft.com/exchange/hybrid-configuration-wizard).

- These instructions assume that Outlook Anywhere is enabled and functional on all the on-premises legacy Exchange public folder servers. For information about how to enable Outlook Anywhere, see [Outlook Anywhere](https://docs.microsoft.com/exchange/outlook-anywhere-exchange-2013-help).

- Implementing legacy public folder coexistence for a hybrid deployment of Exchange with Microsoft 365 or Office 365 may require you to fix conflicts during the import procedure. Conflicts can occur because a non-routable email address that's assigned to mail-enabled public folders, conflicts with other users and groups in Microsoft 365 or Office 365, and other reasons.

- These instructions assume that your Exchange Online organization has been upgraded to a version that supports public folders.

- In Exchange Online, you must be a member of the Organization Management role group. This role group is different from the permissions assigned to you when you subscribe to Exchange Online. For information about how to enable the Organization Management role group, see [Manage role groups in Exchange Online](../../permissions-exo/role-groups.md).

- In Exchange 2010, you must be a member of the Organization Management or Server Management RBAC role groups. For details, see [Add Members to a Role Group](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/dd638143(v=exchg.141))

- To access public folders cross-premises, users must upgrade their Outlook clients to the November 2012 Outlook public update or a later version.

  1. To download the November 2012 Outlook update for Outlook 2010, see [Update for Microsoft Outlook 2010 (KB2687623) 32-Bit Edition](https://www.microsoft.com/download/details.aspx?id=35702).

  2. To download the November 2012 Outlook Update for Outlook 2007, see [Update for Microsoft Office Outlook 2007 (KB2687404)](https://www.catalog.update.microsoft.com/Search.aspx?q=outlook+2007)and download in your preferred language from the dialouge box.

- Outlook 2016 for Mac (and earlier versions) and Outlook for Mac for Office 365 are not supported for cross-premises legacy public folders. Users must be in the same location as the public folders to access them with Outlook for Mac or Outlook for Mac for Office 365. Additionally, users whose mailboxes are in Exchange Online won't be able to access on-premises public folders using Outlook Web App.

- After you follow the instructions in this article to configure your on-premises public folders for a hybrid deployment, users who are external to your organization won't be able to send messages to your on-premises public folders unless you take additional steps. You can either set the accepted domain for the public folders to Internal Relay (see [Manage accepted domains in Exchange Online](../../mail-flow-best-practices/manage-accepted-domains/manage-accepted-domains.md)) or you can disable Directory Based Edge Blocking (DBEB) (see [Use Directory Based Edge Blocking to reject messages sent to invalid recipients](../../mail-flow-best-practices/use-directory-based-edge-blocking.md)).

## Step 2: Make remote public folders discoverable

1. If your public folders are on Exchange 2010 or later servers, you must install the Client Access server (CAS) role on all mailbox servers that have a public folder database. This allows the Microsoft Exchange RpcClientAccess service to be running so that all clients can access public folders. For more information, see [Install Exchange Server 2010](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124778(v=exchg.141)).

   > [!NOTE]
   > This server doesn't have to be part of the Client Access load balancing. For more information, see [Understanding Load Balancing in Exchange 2010](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ff625247(v=exchg.141)).

2. Create an empty mailbox database on each public folder server.

   For Exchange 2010, run the following command. This command excludes the mailbox database from the mailbox provisioning load balancer. This prevents new mailboxes from being added automatically to this database.

   ```PowerShell
   New-MailboxDatabase -Server <PFServerName_with_CASRole> -Name <NewMDBforPFs> -IsExcludedFromProvisioning $true
   ```

   > [!NOTE]
   > We recommend that the only mailbox that you add to this database is the proxy mailbox that you'll create in step 3. No other mailboxes should be created on this mailbox database.

3. Create a proxy mailbox within the new mailbox database, and hide the mailbox from the address book. The SMTP of this mailbox will be returned by AutoDiscover as the _DefaultPublicFolderMailbox_ SMTP, so that by resolving this SMTP the client can reach the legacy exchange server for public folder access.

   ```PowerShell
   New-Mailbox -Name <PFMailbox1> -Database <NewMDBforPFs>
   ```

   ```PowerShell
   Set-Mailbox -Identity <PFMailbox1> -HiddenFromAddressListsEnabled $true
   ```

4. For Exchange 2010, enable AutoDiscover to return the proxy public folder mailboxes.

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

The Directory Synchronization service doesn't synchronize mail-enabled public folders. Running the following script will synchronize the mail-enabled public folders across premises. Special permissions assigned to mail-enabled public folders will need to be recreated in the cloud since cross-premise permission are not supported in Hybrid Deployment scenarios. For more information, see [Exchange Server Hybrid Deployment](https://docs.microsoft.com/exchange/exchange-hybrid).

> [!NOTE]
> Synchronized mail-enabled public folders will appear as mail contact objects for mail flow purposes and will not be viewable in the Exchange admin center. See the Get-MailPublicFolder command. To recreate the SendAs permissions in the cloud, use the Add-RecipientPermission command.

On the legacy Exchange server, run the following command to synchronize mail-enabled public folders from your local on-premises Active Directory to Microsoft 365 or Office 365.

```PowerShell
Sync-MailPublicFolders.ps1 -Credential (Get-Credential) -CsvSummaryFile "<sync_summary.csv>"
```

Where you're prompted for your Microsoft 365 or Office 365 username and password, and _\<sync\_summary.csv\>_ is the path to where you would like to log synchronization operations and errors, in .csv format.

> [!NOTE]
> Before running the script, we recommend that you first simulate the actions that the script would take in your environment by running it as described above with the _WhatIf_ parameter. > We also recommend that you run this script daily to synchronize your mail-enabled public folders.

## Step 5: Configure Exchange Online users to access on-premises public folders

The final step in this procedure is to configure the Exchange Online organization and to allow access to the legacy on-premises public folders.

Enable the exchange online organization to access the on-premises public folders. You will point to all of the proxy public folder mailboxes that you created in [Step 2: Make remote public folders discoverable](#step-2-make-remote-public-folders-discoverable).

Run the following command in **Exchange Online PowerShell**:

```PowerShell
Set-OrganizationConfig -PublicFoldersEnabled Remote -RemotePublicFolderMailboxes 'PFMailbox1','PFMailbox2','PFMailbox3'
```

You must wait until Active Directory synchronization has completed to see the changes. This process can take up to 3 hours to complete. If you don't want to wait for the recurring synchronizations that occur every three hours, you can force directory synchronization at any time. For detailed steps to do force directory synchronization, see [Method 1: Manually verify that the service is started and that the admin account can sign in](https://support.microsoft.com/help/2882421/directory-synchronization-to-azure-active-directory-stops-or-you-re-wa). Microsoft 365 and Office 365 randomly select one of the public folder mailboxes that's supplied in this command.

> [!IMPORTANT]
> A Microsoft 365 or Office 365 user who is not represented by a MailUser object on-premises (local to the target public folder hierarchy) won't be able to access legacy or Exchange 2013 on-premises public folders. See the Knowledge Base article [Exchange Online users can't access legacy on-premises public folders](https://support.microsoft.com/help/3106618) for a solution.

## How do I know this worked?

Log on to Outlook for a user who is in Exchange Online, and then run the following public folder tests:

- View the hierarchy.

- Check permissions.

- Create and delete public folders.

- Post content to and delete content from a public folder.
