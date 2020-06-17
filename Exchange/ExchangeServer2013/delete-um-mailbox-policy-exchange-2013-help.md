---
title: 'Delete a UM mailbox policy: Exchange 2013 Help'
TOCTitle: Delete a UM mailbox policy
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c8758464-3c52-4dd3-b2a6-142a99bb0628
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Delete a UM mailbox policy in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you delete a Unified Messaging (UM) mailbox policy, the UM mailbox policy will no longer be available to be associated with recipients who are being enabled for UM. You can't delete a UM mailbox policy if it's referenced by any UM-enabled mailboxes, and you can't delete a UM dial plan if a UM mailbox policy is associated with it.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to delete a UM mailbox policy

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM dial plan** page, under **UM Mailbox Policies**, on the toolbar, click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif).

## Use the Shell to delete a UM mailbox policy

This example deletes a UM mailbox policy named `MyUMMailboxPolicy`.

```powershell
Remove-UMMailboxPolicy -Identity MyUMMailboxPolicy
```
