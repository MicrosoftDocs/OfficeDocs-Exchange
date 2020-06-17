---
title: 'Export and import retention tags: Exchange 2013 Help'
TOCTitle: Export and import retention tags
ms:assetid: 18405ea2-7ccc-475e-bd84-8b040e17bf44
ms:mtpsurl: https://technet.microsoft.com/library/JJ907307(v=EXCHG.150)
ms:contentKeyID: 50639770
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Export and import retention tags

_**Applies to:** Exchange Server 2013_

There are several scenarios in which you may want to export or import retention tags, including:

- Applying the same retention policies across all servers in a multi-forest Exchange organization

- Applying the same retention policies in a hybrid deployment where some mailboxes reside in your on-premises Exchange organization and some reside in Exchange Online.

- Applying retention policies in an Exchange Archiving scenario, where users with on-premises Exchange 2010 or later mailboxes have a cloud-based archive.

In these scenarios, the Managed Folder Assistant can correctly process an item that has a retention tag applied after the item or the mailbox is moved to another organization.

> [!WARNING]
> To keep retention tags and retention policies synchronized between two organizations, every time you make changes to a retention tag or policy in the source organization, you must perform this procedure to export retention tags and policies from the source organization and import them in the destination organization.<BR>You can't select specific retention tags or policies to export. The Export-RetentionTags.ps1 script exports all retention tags and policies from an organization.

For additional management tasks related to Messaging Records Management, see [Messaging Records Management Procedures](https://docs.microsoft.com/office365/securitycompliance/inactive-mailboxes-in-office-365).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging Records Management" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- The procedures in this topic depend on these three files in the `%ExchangeInstallPath%Scripts` folder on yourExchange server:

  - Export-RetentionTags.ps1

  - Import-RetentionTags.ps1

  - MigrateRetentionTags.strings.psd1

- You can't select specific retention tags or policies to export or import. The Export-RetentionTags.ps1 script exports all retention tags and policies from an organization. The Import-RetentionTags.ps1 script imports all retention tags and policies in the XML file being imported, replacing all existing retention tags and policies in an Exchange organization.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Export retention tags from an on-premises Exchange organization

1. Run this Exchange Management Shell command to change directory to the **Scripts** subdirectory in your Exchange installation path.

   ```powershell
   Cd $Env:ExchangeInstallPath\Scripts
   ```

2. Run the Export-RetentionTags.ps1 script to export retention tags to an XML file.

   > [!IMPORTANT]
   > If you're importing or exporting retention tags and retention policies to Exchange Online, you must connect your Windows PowerShell session to Exchange Online. For details, see <A href="https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell">Connect to Exchange Online using remote PowerShell</A>.

   ```powershell
   .\Export-RetentionTags.ps1 "c:\docs\ExportedRetentionTags.xml"
   ```

## How do you know this worked?

To verify that you have successfully exported retention tags and retention policies, do the following:

1. Navigate to the path you specified in the command to export and verify that the XML file with the name you specified has been created.

2. Optionally, you can open the XML file in a text editor to review its contents.

## Step 2: Import retention tags to an Exchange organization

1. Run this Exchange Management Shell command to change the directory to the **Scripts** subdirectory in your Exchange installation path.

   ```powershell
   Cd $Env:ExchangeInstallPath\Scripts
   ```

2. Run the Import-RetentionTags.ps1 script to import retention tags from a previously exported XML file.

   > [!IMPORTANT]
   > If you're importing or exporting retention tags and retention policies to Exchange Online, you must connect your Windows PowerShell session to Exchange Online. For details, see <A href="https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell">Connect to Exchange Online using remote PowerShell</A>. <br>/<br/> When running this script against Exchange Online, you may be prompted to confirm that you want to run software from an untrusted publisher. Verify that the name of the publisher appears as <CODE>CN=Microsoft Corporation, OU=MOPR, O=Microsoft Corporation, L=Redmond, S=Washington, C=US</CODE>, and then click <STRONG>R</STRONG> to allow the script to be run once or <STRONG>A</STRONG> to always run.

   ```powershell
   .\Import-RetentionTags.ps1 "c:\docs\ExportedRetentionTags.xml"
   ```

## How do you know this worked?

To verify that you have successfully imported retention tags and retention policies, do the following:

1. In the EAC, navigate to **Compliance Management** \> **Retention tags**, and verify that the retention tags have been imported successfully. Navigate to **Compliance Management** \> **Retention policies**, and verify that the retention policies have been imported successfully.

2. Use the **Get-RetentionPolicy** and **Get-RetentionPolicyTag** cmdlets to verify that the tags and policies have been created. For an example about how to retrieve retention tags and retention policies, see Examples in [Get-RetentionPolicyTag](https://docs.microsoft.com/powershell/module/exchange/Get-RetentionPolicyTag) and [Get-RetentionPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-RetentionPolicy).
