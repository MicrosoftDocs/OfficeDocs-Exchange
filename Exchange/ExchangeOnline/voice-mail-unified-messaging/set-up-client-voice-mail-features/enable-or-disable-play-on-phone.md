---
localization_priority: Normal
description: You can enable or disable the Play on Phone feature for users associated with a Unified Messaging (UM) mailbox policy. This option is enabled by default and allows users to play their voice mail messages over any phone. This option isn't available to UM-enabled users who have a mailbox on a Microsoft Exchange Server 2007 server.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d3281a97-6fc6-42a3-855f-1af1184a644a
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable Play on Phone for Outlook Voice Access users in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable Play on Phone for Outlook Voice Access users in Exchange Online

You can enable or disable the Play on Phone feature for users associated with a Unified Messaging (UM) mailbox policy. This option is enabled by default and allows users to play their voice mail messages over any phone. This option isn't available to UM-enabled users who have a mailbox on a Microsoft Exchange Server 2007 server.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](../../voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable or disable Play on Phone

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page, select or clear the check box next to **Allow Play on Phone for voice mail**.

4. Click **Save**.

## Use Exchange Online PowerShell to enable or disable Play on Phone

This example enables the Play on Phone feature for users who are associated with the UM mailbox policy `MyUMMailboxPolicy`.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowPlayOnPhone $true
```

This example disables the Play on Phone feature for users who are associated with the UM mailbox policy `MyUMMailboxPolicy`.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowPlayOnPhone $false
```
