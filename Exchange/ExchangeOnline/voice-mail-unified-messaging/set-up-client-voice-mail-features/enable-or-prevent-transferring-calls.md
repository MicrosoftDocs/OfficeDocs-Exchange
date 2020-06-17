---
localization_priority: Normal
description: You can enable Outlook Voice Access users to transfer calls to a user who's associated with a Unified Messaging (UM) dial plan, or prevent them from doing so. By default, both this option and the Leave voice messages without ringing a user's phone option are enabled, so that Outlook Voice Access users can transfer calls to users in the same UM dial plan and leave voice messages for them. This setting only applies to Outlook Voice Access users who have entered their PIN and are authenticated.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b80c57f1-394c-4608-8ad3-52a3e6d697db
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or prevent transferring calls from Outlook Voice Access in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or prevent transferring calls from Outlook Voice Access in Exchange Online

You can enable Outlook Voice Access users to transfer calls to a user who's associated with a Unified Messaging (UM) dial plan, or prevent them from doing so. By default, both this option and the **Leave voice messages without ringing a user's phone** option are enabled, so that Outlook Voice Access users can transfer calls to users in the same UM dial plan and leave voice messages for them. This setting only applies to Outlook Voice Access users who have entered their PIN and are authenticated.

For additional tasks related to UM dial plans, see  [UM dial plan procedures in Exchange Online](../connect-voice-mail-system/um-dial-plan-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable or prevent Outlook Voice Access users from transferring calls

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan that you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, click **Configure**.

3. In **transfer & search**, under **Allow callers to**, select the check box next to **transfer to users** to enable callers to transfer calls to other users within the dial plan. If you want to prevent Outlook Voice Access users from transferring calls to users, clear this check box.

4. Click **Save**.

## Use Exchange Online PowerShell to enable or prevent Outlook Voice Access users from transferring calls

This example enables Outlook Voice Access users to transfer calls to users in the same dial plan on a UM dial plan named `MyUMDialPlan`.

```PowerShell
Set-UMDialPlan -identity MyUMDialPlan -AllowDialPlanSubscribers $true
```

This example prevents Outlook Voice Access users from transferring calls to users in the same dial plan on a UM dial plan named `MyUMDialPlan`.

```PowerShell
Set-UMDialPlan -identity MyUMDialPlan -AllowDialPlanSubscribers $false
```
