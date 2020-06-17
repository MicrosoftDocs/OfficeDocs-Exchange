---
localization_priority: Normal
description: You can enable Outlook Voice Access users to send voice mail messages to other UM-enabled users who are associated with the same dial plan, or prevent them from doing so.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 63544ae2-6a28-40b2-82fc-3df83e93ee56
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable sending voice messages from Outlook Voice Access in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable sending voice messages from Outlook Voice Access in Exchange Online

You can enable Outlook Voice Access users to send voice mail messages to other UM-enabled users who are associated with the same dial plan, or prevent them from doing so.

By default, this setting is enabled. If you disable this setting, Outlook Voice Access users that call into an Outlook Voice Access number won't be able to send voice messages to users within the same dial plan.

For additional tasks related to UM dial plans, see  [UM dial plan procedures in Exchange Online](../connect-voice-mail-system/um-dial-plan-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable or prevent Outlook Voice Access users sending voice messages to users in the same dial plan

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Transfer & search**, under **Allow callers to**, select **Leave voice messages without ringing a user's phone** to allow sending voice messages. If you want to prevent sending voice messages for users, clear this setting.

5. Click **Save**.

## Use Exchange Online PowerShell to enable or prevent Outlook Voice Access users sending voice messages to users in the same dial plan

This example enables Outlook Voice Access users associated with the UM dial plan named `MyUMDialPlan` to send voice messages to users associated with the same dial plan.

```PowerShell
Set-UMDialPlan -identity MyUMDialPlan -SendVoiceMsgEnabled $true
```

This example prevents Outlook Voice Access users associated with the UM dial plan named `MyUMDialPlan` from sending voice messages to users associated with the same dial plan.

```PowerShell
Set-UMDialPlan -identity MyUMDialPlan -SendVoiceMsgEnabled $false
```
