---
localization_priority: Normal
description: When you delete a Unified Messaging (UM) mailbox policy, the UM mailbox policy will no longer be available to be associated with recipients who are being enabled for UM. You can't delete a UM mailbox policy if it's referenced by any UM-enabled mailboxes, and you can't delete a UM dial plan if a UM mailbox policy is associated with it.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c8758464-3c52-4dd3-b2a6-142a99bb0628
ms.reviewer: 
f1.keywords:
- NOCSH
title: Delete a UM mailbox policy in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Delete a UM mailbox policy in Exchange Online

When you delete a Unified Messaging (UM) mailbox policy, the UM mailbox policy will no longer be available to be associated with recipients who are being enabled for UM. You can't delete a UM mailbox policy if it's referenced by any UM-enabled mailboxes, and you can't delete a UM dial plan if a UM mailbox policy is associated with it.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to delete a UM mailbox policy

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM dial plan** page, under **UM Mailbox Policies**, on the toolbar, click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif).

## Use Exchange Online PowerShell to delete a UM mailbox policy

This example deletes a UM mailbox policy named `MyUMMailboxPolicy`.

```PowerShell
Remove-UMMailboxPolicy -Identity MyUMMailboxPolicy
```
