---
localization_priority: Normal
description: 'Summary: Learn about supported limits for public folders in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 709b075e-9584-484b-bcaa-e781c26497b4
ms.reviewer:
title: Limits for public folders
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Limits for public folders

In Exchange Server, public folders are based on a mailbox architecture that allows public folders to benefit from things such as the resiliency of a Database Availability Group (DAG) and other mailbox enhancements. However, there are limits and performance considerations that should be taken into account.

## Limits

The following table lists the limits for public folders in on-premises Exchange Server. Unless the limits are specifically stated as recommended, the values listed in this table are the supported limits for public folders.

> [!IMPORTANT]
> Looking for Exchange Online limits for Office 365? See [Exchange Online Limits](https://go.microsoft.com/fwlink/p/?LinkID=391188).

|**Item**|**Limits**|**Notes**|
|:-----|:-----|:-----|
|Total number of public folder mailboxes|1,000|1,000 is the limit for Exchange Server 2016 CU2 or later. Although you can create more than 1,000 public folder mailboxes, it isn't supported. [Create a public folder mailbox](create-public-folder-mailboxes.md)|
|Total public folders in hierarchy|1,000,000|Although you can create more than 1,000,000 public folders, it isn't supported. For any deployment of 100,000 or more public folders, we recommend reading [Considerations when deploying public folders](deployment-considerations.md).|
|Sub-folders under the parent folder|10,000|While you can create more than 1,000 sub-folders under a parent folder, we don't recommend that you do so._FolderHierarchyChildrenCountReceiveQuota_ parameter on the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/mailboxes/set-mailbox) cmdlet.|
|Folder depth|300|The folder depth is the number levels of nested folders that can exist in one branch of a public folder tree. _FolderHierarchyDepthRecieveQuota_ parameter on the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/mailboxes/set-mailbox) cmdlet.|
|Maximum messages per public folder|1 million| _MailboxMessagesPerFolderCountRecieveQuota_ parameter on the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/mailboxes/set-mailbox) cmdlet.|
|Maximum individual public folder size|10 GB|This limit doesn't include subfolders beneath a single folder.[Configure storage quotas for a mailbox](../../recipients/user-mailboxes/storage-quotas.md)|
|Public folder mailbox size|100 GB|[Configure storage quotas for a mailbox](../../recipients/user-mailboxes/storage-quotas.md)|
|Number of user logons per public folder mailbox|2,000 concurrent user logons|We recommend that you configure your hierarchy so that you have no more than 2,000 users per public folder mailbox. For example, if you have 20,000 users, you should have 10 public folder mailboxes.|
|Moved item retention|14 days recommended|Use the _DefaultPublicFolderMovedItemRetention_ parameter on the **Set-OrganizationConfig** cmdlet.|
|Age limit|We recommend that you set this as the same default that you use for regular mailboxes.|These settings can be set at the following levels:**Organizational level**: The _DefaultPublicFolderAgeLimit_ parameter on the **Set-OrganizationConfig** cmdlet.**Folder level**: The _AgeLimit_ parameter on the **Set-PublicFolder** cmdlet.|
|Deleted item retention|We recommend that you set this as the same default that you use for regular mailboxes.|These settings can be set at the following levels:**Organizational level**: The _DefaultPublicFolderMovedItemRetention_ parameter on the **Set-OrganizationConfig** cmdlet.**Mailbox level**: The _RetainDeletedItemsFor_ on the **Set-Mailbox** cmdlet.**Folder level:**: The _RetainDeleteItemsFor_ parameter on the **Set-PublicFolder** cmdlet.|
|Maximum number of public folders that can be migrated from Exchange 2010 to Exchange 2016|500,000|Although you can attempt to migrate more than 500,000 public folders, it isn't supported. For details on migrating public folders, see [Use batch migration to migrate public folders from Exchange 2010 to Exchange 2016](batch-migration-from-previous-versions.md).|
