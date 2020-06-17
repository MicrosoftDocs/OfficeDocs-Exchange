---
description: 'Summary: Follow these steps to return your public folder infrastructure to its pre-migration state in your Exchange Server on-premises organization.'
localization_priority: Normal
ms.author: v-mapenn
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.assetid: bcd54aa0-aa45-4c68-b504-1475842d4b96
ms.collection: exchange-server
ms.reviewer: 
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
title: Roll back a public folder migration from Exchange Server to Exchange Online

---

# Roll back a public folder migration from Exchange Server to Exchange Online

If you run into issues with your public folder migration to Exchange Online, or for any other reason need to reactivate your Exchange Server public folders, follow the steps below.

## Roll back the migration

Note that if you roll back your migration, you will lose any content that was added to public folders in Exchange Online post-migration, either through clients or via email for mail-enabled public folders. To save this content, you can export the post-migration public folder content to a .pst file, which can then be imported into the on-premises public folders when the rollback is complete.

1. In your Exchange on-premises environment, run the following command to unlock your Exchange Server public folders (note that the unlocking may take several hours):

   ```PowerShell
   Set-OrganizationConfig -PublicFolderMailboxesLockedForNewConnections:$false -PublicFolderMailboxesMigrationComplete:$false -PublicFoldersEnabled Local
   ```

2. In your Exchange on-premises environment, revert the `ExternalEmailAddress` of any mail-enabled public folder that was updated by SetMailPublicFolderExternalAddress.ps1 (the script used in *Step 8: Test and unlock public folders in Exchange Online* of [Use batch migration to migrate Exchange Server public folders to Exchange Online](migrate-to-exchange-online.md). You can refer to the summary file created by the script to identify the ones that were modified, or use the file OnPrem_MEPF.xml file generated earlier in the same batch migration process to get the original properties for all mail-enabled public folders.

3. In Exchange Online PowerShell, run the following commands to remove all Exchange Online public folders and mailboxes:

   ```PowerShell
   Get-MailPublicFolder -ResultSize Unlimited | where {$_.EntryId -ne $null}| Disable-MailPublicFolder -Confirm:$false
   Get-PublicFolder -GetChildren \ -ResultSize Unlimited | Remove-PublicFolder -Recurse -Confirm:$false
   $hierarchyMailboxGuid = $(Get-OrganizationConfig).RootPublicFolderMailbox.HierarchyMailboxGuid
   Get-Mailbox -PublicFolder | Where-Object {$_.ExchangeGuid -ne $hierarchyMailboxGuid} | Remove-Mailbox -PublicFolder -Confirm:$false -Force
   Get-Mailbox -PublicFolder | Where-Object {$_.ExchangeGuid -eq $hierarchyMailboxGuid} | Remove-Mailbox -PublicFolder -Confirm:$false -Force
   Get-Mailbox -PublicFolder -SoftDeletedMailbox | Remove-Mailbox -PublicFolder -PermanentlyDelete:$true
   ```

4. Run the following command in your Exchange Online environment to redirect public folder traffic back to on-premises (Exchange Server):

   ```PowerShell
   Set-OrganizationConfig -PublicFoldersEnabled Remote
   ```

5. See [Configure Exchange 2013 public folders for a hybrid deployment](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/set-up-modern-hybrid-public-folders) for instructions on reconfiguring access to your on-premises public folders, so that your Exchange Online users can access them.
