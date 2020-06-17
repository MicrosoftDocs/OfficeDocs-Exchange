---
title: 'Configure archive quotas for an In-Place Archive in Exchange 2013'
TOCTitle: Configure archive quotas for an In-Place Archive in Exchange 2013
ms:assetid: f10e77c7-e1d4-415a-bef9-cb3f00e74c34
ms:mtpsurl: https://technet.microsoft.com/library/Ee633489(v=EXCHG.150)
ms:contentKeyID: 50470879
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure archive quotas for an In-Place Archive in Exchange 2013

_**Applies to:** Exchange Server 2013_

In on-premises deployments, In-Place Archives are created with unlimited storage quotas by default. As a result, you'll need to edit a mailbox's properties to set storage quotas for the archive. You can set the following quotas for an archive:

- **Archive warning quota**: When an In-Place Archive exceeds the specified archive warning quota, an event is logged for the Exchange administrator and a warning message is sent to the mailbox user.

- **Archive quota**: When an In-Place Archive exceeds the specified archive quota, messages are no longer moved to the archive and a warning message is sent to the mailbox user.

To learn more about In-Place Archives, see [In-Place Archiving in Exchange 2013](in-place-archiving-in-exchange-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- In the Exchange admin center (EAC), you can use a drop-down list with fixed values to configure the archive quota and archive warning quota. If you want to set either quota to a value that's not listed in the EAC, use the Shell.

- Configure the archive warning quota to a lower value than the archive quota. Depending on the rate of archive growth for a user, the difference between the archive warning quota and the archive quota should allow for sufficient time for the user to take appropriate actions, such as deleting items from the archive or requesting that an administrator or IT helpdesk to raise the archive quota.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure the archive quota and archive warning quota for a mailbox

1. Navigate to **Recipients** \> **Mailboxes**

2. In the list view, select a mailbox,

3. In the details pane, under **In-Place Archive**, click **View details**.

4. In **Archive Mailbox**, use the **Quota value (GB)** and **Issue warning at (GB)** lists to select the desired values.

5. Click **OK**.

## Use the Shell to configure the archive quota and archive warning quota for a mailbox

This example sets Chris Ashton's mailbox archive quota to 10 gigabyte (GB), at which time the user will receive a warning message that the In-Place Archive is full and he will no longer be able to move items to the archive. This example also sets the archive warning quota to 9.5 GB, at which time the user will receive a warning message that the In-Place Archive is almost full.

```powershell
Set-Mailbox -Identity "Chris Ashton" -ArchiveQuota 10GB -ArchiveWarningQuota 9.5GB
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).

## How do you know this worked?

To verify that you've successfully enabled an on-premises archive for an existing mailbox, do one of the following:

- In the EAC, navigate to **Recipients**  \> **Mailboxes** and select the mailbox you want. In the details pane, under **In-Place Archive**, click **View Details** and verify the archive's quota settings.

- In the Shell, run the following command to display quota information about the archive.

  ```powershell
  Get-Mailbox <Name> | FL Name,Archive*Quota
  ```
