---
title: "Configure Exchange Server public folders for a hybrid deployment"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Strat_EX_EXOBlocker
- Ent_O365_Hybrid
- Hybrid
- M365-email-calendar
ms.assetid: b828520f-022c-4fcb-ab68-e1c330e87c33
ms.reviewer:
description: "Summary: Instructions for enabling Exchange Online users to access on-premises public folders in your Exchange 2013 environment."
---

# Configure Exchange Server public folders for a hybrid deployment

 **Summary**: Instructions for enabling Exchange Online users to access on-premises public folders in your Exchange 2013, Exchange 2016, or Exchange 2019 environment.

In a hybrid deployment, your users can be in Exchange Online, on-premises, or both, and your public folders are either in Exchange Online or on-premises. Sometimes your online users may need to access public folders in your Exchange Server on-premises environment.

> [!NOTE]
> If you have Exchange 2010 public folders, see [Configure legacy on-premises public folders for a hybrid deployment](set-up-legacy-hybrid-public-folders.md).

This article describes how to enable your Exchange Online, Microsoft 365, or Office 365 users to access public folders in Exchange 2013, Exchange 2016 and Exchange 2019 (for the rest of this article, referred to as *Exchange Server*). To enable on-premises Exchange Server users to access public folders in Exchange Online, Microsoft 365, or Office 365, see [Configure Exchange Online public folders for a hybrid deployment](set-up-exo-hybrid-public-folders.md).

An Exchange Online, Microsoft 365, or Office 365 user must be represented by a MailUser object in the Exchange on-premises environment in order to access Exchange Server public folders. This MailUser object must also be local to the target Exchange Server public folder hierarchy. If you have Exchange Online, Microsoft 365, or Office 365 users who aren't currently represented on-premises by MailUser objects, refer to the Microsoft Knowledge Base article [KB3106618](https://support.microsoft.com/help/3106618) to create matching on-premises entities.

## What do you need to know before you begin?

1. These instructions assume that Azure Active Directory Connect synchronization services (Azure AD Connect sync) is configured to synchronize public folder mailbox objects to Exchange Online. Ensure that your public folder mailbox objects are synchronized to Exchange Online and that they have auto-discoverable primary SMTP addresses.

    Here is an example of proper configuration in an on-premises environment:

:::image type="content" source="../media/hybrid-pfs-on-prem-config.png" alt-text="public folder synchronization in Exchange Server":::   

Here is an example of proper configuration in Exchange Online:

:::image type="content" source="../media/hybrid-pfs-exo-config.png" alt-text="Public folder synchronization in Exchange Online":::  

2. These instructions assume that you have used the Hybrid Configuration wizard to configure and synchronize your on-premises and Exchange Online environments and that the DNS records used for most users' AutoDiscover references an on-premises end-point. For more information, see [Hybrid Configuration wizard](../hybrid-configuration-wizard.md).

3. The public folders in this configuration cannot be accessed using Outlook Web App (Exchange 2013) or Outlook on the web (Exchange 2016 and Exchange 2019).

4. Implementing public folder coexistence for a hybrid deployment of Exchange with Office 365 may require you to fix conflicts during the import procedure. Conflicts can happen due to non-routable email addresses assigned to mail enabled public folders, conflicts with other users and groups in Office 365, and other attributes.

5. In order to access public folders cross-premises, users must upgrade their Outlook clients to the November 2012 Outlook public update or later.

6. To download the November 2012 Outlook update for Outlook 2010, see [Update for Microsoft Outlook 2010 (KB2687623) 32-Bit Edition](https://www.microsoft.com/download/details.aspx?id=35702).

7. Outlook 2011 for Mac and Outlook for Mac for Office 365 are not supported for cross-premises public folders. Users must be in the same location as the public folders to access them with Outlook 2011 for Mac or Outlook for Mac for Office 365. In addition, users whose mailboxes are in Exchange Online won't be able to access on-premises public folders using Outlook Web App or Outlook on the web.

    > [!NOTE]
    > Outlook 2016 for Mac is supported for cross-premises public folders. If clients in your organization use Outlook 2016 for Mac, make sure they have installed the April 2016 update. Otherwise, those users will not be able to access public folders in a hybrid topology. For more information, see [Accessing public folders with Outlook 2016 for Mac](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/access-public-folders-with-outlook-2016-for-mac).

## Step 1: Download the scripts

1. Download the following files from [Exchange 2013/2016 Public Folders Migration Scripts](https://www.microsoft.com/download/details.aspx?id=54855):

   - `Sync-ModernMailPublicFolders.ps1`

   - `Sync-ModernMailPublicFolders.psd1`
   
    > [!NOTE]
    > The download package at this location contains additional files. To follow the instructions in this article, you only need the two listed above. 

2. Save the files to the local computer on which you'll be running PowerShell. For example, C:\PFScripts.

## Step 2: Synchronize mail-enabled public folder objects to Exchange Online

Azure AD Connect sync doesn't synchronize mail-enabled public folders to Exchange Online. Running the following script will synchronize the mail-enabled public folders across your on-premises environment and Exchange Online. Special permissions assigned to mail-enabled public folders, such as **Send As**, will need to be recreated in Office 365 since cross-premise permissions are not supported in hybrid deployment scenarios. For more information, see [Exchange hybrid deployment documentation](../exchange-hybrid.md#exchange-hybrid-deployment-documentation).

> [!NOTE]
> Synchronized mail-enabled public folders will not be visible in the Exchange admin center (EAC). Instead, use the [Get-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/get-mailpublicfolder) cmdlet. To recreate Send As permissions in the cloud, use the [Add-RecipientPermission](https://docs.microsoft.com/powershell/module/exchange/add-recipientpermission) cmdlet.

On the Exchange server, run the following command to synchronize mail-enabled public folders from your local on-premises Active Directory to Office 365.

```PowerShell
.\Sync-ModernMailPublicFolders.ps1 -Credential (Get-Credential) -CsvSummaryFile:sync_summary.csv
```

Where `Credential` is your Microsoft 365 or Office 365 admin username and password, and `CsvSummaryFile` is the path to where you would like to log synchronization operations and errors, in .csv format.

> [!NOTE]
> Before running the script, we recommend that you first simulate the actions that the script would take in your environment by running it as described above with the `-WhatIf` switch. <br/> We also recommend that you run this script daily to synchronize your mail-enabled public folders.

Use the steps in [Troubleshooting mail enabled public folder synchronization failures when using PowerShell script](https://docs.microsoft.com/exchange/troubleshoot/public-folders/mepf-sync-failures-script) if you see errors while running the script. 

## Step 3: Configure Exchange Online users to access Exchange Server on-premises public folders

The final step in this process is to configure the Exchange Online organization and to allow access to the Exchange Server public folders.

Enable the Exchange Online organization to access the on-premises public folders. You will point to all of your on-premises public folder mailboxes.

```PowerShell
Set-OrganizationConfig -PublicFoldersEnabled Remote -RemotePublicFolderMailboxes PFMailbox1,PFMailbox2,PFMailbox3
```
> [!NOTE]
> You must wait until Azure Active Directory (AAD) synchronization is complete before you can see the changes. This process can take up to three hours to complete. If you don't want to wait for the recurring synchronizations that occur every three hours, you can force directory synchronization at any time. For detailed steps to do force directory synchronization, see [Azure AD Connect sync: Scheduler](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler).

## How do I know this worked?

Use following Exchange Online PowerShell command to verify if Exchange Online mailboxes have been assigned an **EffectivePublicFolderMailbox** value:

```PowerShell
Get-Mailbox |ft name,EffectivePublicFolderMailbox
```
Next, log on to Outlook with the credentials of an Exchange Online user and perform the following public folder tests:

- View the hierarchy

- Check permissions

- Create and delete public folders

- Post content to and delete content from a public folder

You can use [this article](https://support.microsoft.com/help/4549862/outlook-or-owa-cannot-connect-to-public-folders) to troubleshoot any Outlook connectivity issues with your public folders.  
