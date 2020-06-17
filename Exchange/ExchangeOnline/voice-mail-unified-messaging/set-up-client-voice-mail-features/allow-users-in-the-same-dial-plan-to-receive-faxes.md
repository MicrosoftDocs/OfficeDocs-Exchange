---
localization_priority: Normal
description: You can enable all users who are linked with a Unified Messaging (UM) dial plan to receive fax messages in their mailboxes. By default, users who are enabled for Unified Messaging and are linked with a UM dial plan can receive fax messages. To allow UM-enabled users to receive fax messages in their mailboxes, the dial plan must be configured to accept incoming fax calls. You must also enable faxing on the UM mailbox policy and for the user. By default, faxing is enabled on dial plans, UM mailbox policies, and for users. However, there may be times when these default settings have changed and UM-enabled users can't receive fax messages.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: cb245028-0b86-4171-879e-934dd35fa626
ms.reviewer: 
f1.keywords:
- NOCSH
title: Allow users in the same dial plan to receive faxes in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Allow users in the same dial plan to receive faxes in Exchange Online

You can enable all users who are linked with a Unified Messaging (UM) dial plan to receive fax messages in their mailboxes. By default, users who are enabled for Unified Messaging and are linked with a UM dial plan can receive fax messages. To allow UM-enabled users to receive fax messages in their mailboxes, the dial plan must be configured to accept incoming fax calls. You must also enable faxing on the UM mailbox policy and for the user. By default, faxing is enabled on dial plans, UM mailbox policies, and for users. However, there may be times when these default settings have changed and UM-enabled users can't receive fax messages.

If you prevent fax messages from being received on a dial plan, all users who are associated with the dial plan won't be able to receive fax messages, even if you configure an individual user's properties to allow them to receive fax messages. Enabling or disabling faxing on a UM dial plan takes precedence over the settings for faxing on a UM mailbox policy or an individual UM-enabled user.

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

## Use Exchange Online PowerShell to allow users who are linked to a dial plan to receive faxes

This example enables UM-enabled users who are linked with the UM dial plan named `MyUMDialPlan` to receive incoming faxes.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -FaxEnabled $true
```
