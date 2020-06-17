---
localization_priority: Normal
description: You can prevent UM-enabled users who are linked with a Unified Messaging (UM) dial plan from receiving fax messages. By default, users who are enabled for Unified Messaging and are linked with a UM dial plan can receive fax messages. However, there may be times when you want to prevent users who are associated with a specific UM dial plan from receiving faxes.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4fc66414-c950-4bca-ac20-4e489f288d06
ms.reviewer: 
f1.keywords:
- NOCSH
title: Prevent users in the same dial plan from receiving faxes in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Prevent users in the same dial plan from receiving faxes in Exchange Online

You can prevent UM-enabled users who are linked with a Unified Messaging (UM) dial plan from receiving fax messages. By default, users who are enabled for Unified Messaging and are linked with a UM dial plan can receive fax messages. However, there may be times when you want to prevent users who are associated with a specific UM dial plan from receiving faxes.

You can prevent UM-enabled users from receiving faxes by configuring the UM dial plan, the UM mailbox policy, or the UM-enabled user's mailbox. If you disable incoming fax message delivery on a UM dial plan, all users who are associated with the dial plan will be prevented from receiving fax messages. Enabling or disabling faxing on a UM dial plan takes precedence over the settings for an individual UM-enabled user.

> [!NOTE]
> You can use the EAC to configure fax settings on a UM mailbox policy. However, you must use Exchange Online PowerShell to configure fax settings on dial plans or for individual users.

For more information about fax partners, see [Microsoft solution providers](https://www.microsoft.com/solution-providers/).

For additional management tasks related to faxing, see [Faxing procedures](faxing-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to prevent users who are linked to a dial plan from receiving faxes

This example prevents UM-enabled users associated with the UM dial plan named `MyUMDialPlan` from receiving faxes.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -FaxEnabled $false
```
