---
localization_priority: Normal
description: Learn how to create a public folder mailbox in Exchange 2016 or Exchange 2019.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 64437ffd-231b-4c10-84df-232ccbe9538f
ms.reviewer:
title: Create a public folder mailbox in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create a public folder mailbox in Exchange Server

Before you can create a public folder in Exchange server, you must first create a public folder mailbox. Public folder mailboxes contain the hierarchy information as well as the content for public folders.

The first public folder mailbox that you create in the organization is the primary hierarchy mailbox, which contains the only writable copy of the public folder hierarchy. Any additional public folder mailboxes that you create are secondary hierarchy mailboxes, which contain a read-only copy of the public folder hierarchy. You can create multiple public folder mailboxes for load balancing.

> [!NOTE]
> For more information about the storage quotas and limits for public folders in on-premises Exchange, see [Limits for public folders](limits.md).

For additional management tasks related to public folders in Exchange Server, see [Public folder procedures](procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: less than 5 minutes.

- Public folders on Exchange 2010 servers can't exist in the same organization with Exchange 2016 or later public folders. If you try to create a public folder mailbox when you still have legacy public folders, you'll receive the error **An existing Public Folder deployment has been detected. To migrate existing Public Folder data, create new Public Folder mailbox using -HoldForMigration switch.**

    Before you can create public folders in Exchange Server 2016 or later, you need to migrate your Exchange 2010 public folders by following the steps in [Use batch migration to migrate public folders from Exchange 2010 to Exchange 2016](batch-migration-from-previous-versions.md).

- To move your public folder mailboxes from Exchange 2013 to Exchange 2016 or Exchange 2019, see [Migrate public folders from Exchange 2013 to Exchange 2016 or Exchange 2019](migrate-from-exchange-2013.md).

- For more information about the Exchange admin center, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in the [Sharing and collaboration permissions](../../permissions/feature-permissions/sharing-and-collaboration-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create a public folder mailbox

1. In the EAC, go to **Public folders** \> **Public folder mailboxes**, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

2. In the **New public folder mailbox** page that opens, enter the following information:

    - **Name**: Enter the name for the public folder mailbox.

    - **Organizational unit**: Click **Browse** to select the location in Active Directory where the mailbox object is created.

    - **Mailbox database**: Click **Browse** to select the mailbox database where the mailbox is created.

    When you're finished, click **Save**.


## Use the Exchange Management Shell to create a public folder mailbox

To create a public folder mailbox, use the following syntax:

```PowerShell
New-Mailbox -PublicFolder -Name <Name>
```

This example creates the primary hierarchy public folder mailbox named Master Hierarchy, because this is the first public folder mailbox in the organization (the value of the _Name_ parameter doesn't determine whether the mailbox is the primary hierarchy public folder mailbox).

```PowerShell
New-Mailbox -PublicFolder -Name "Master Hierarchy"
```

This example creates a secondary hierarcy public folder mailbox named Istanbul, because this isn't the first public folder mailbox in the organization (the value of the _Name_ parameter doesn't determine whether the mailbox is a secondary hierarchy public folder mailbox).

```PowerShell
New-Mailbox -PublicFolder -Name Istanbul
```

For detailed syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/new-mailbox).

## How do you know this worked?

To verify that you've successfully created the a public folder mailbox, do any of these steps:

- In the EAC, go to **Public folders** \> **Public folder mailboxes** and verify the public folder mailbox is listed. The primary hierarcy public folder mailbox has the value **Primary Hierarchy** for the **Contains** property. All other public folder mailboxes have the value **Secondary Hierarchy** for the **Contains** property.

- In the Exchange Management Shell, run the following command to verify the mailbox is listed, and check the value of the **IsRootPublicFolderMailbox** property to see if the mailbox is the primary hierarchy public folder mailbox (`True`) or a secondary hierarchy public folder mailbox (`False`):

    ```PowerShell
    Get-Mailbox -PublicFolder | Format-Table -Auto Name,ServerName,Database,IsRootPublicFolderMailbox
    ```

- In the Exchange Management Shell, run the following commands to verify the primary hierarchy public folder mailbox:

    1. Run the following command:

      ```PowerShell
      Get-OrganizationConfig | Format-List RootPublicFolderMailbox
      ```

    2. Use the GUID value returned by the first command with **Get-Mailbox** to confirm the mailbox name. You can copy the GUID value by right-clicking in the Exchange Management Shell window, selecting **Mark**, highlighting the GUID value, and then pressing ENTER.

      ```PowerShell
      Get-Mailbox -PublicFolder -Identity <GUID>
      ```
