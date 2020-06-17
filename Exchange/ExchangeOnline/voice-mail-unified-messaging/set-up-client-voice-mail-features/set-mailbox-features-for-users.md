---
localization_priority: Normal
description: "Outlook Voice Access contains two interfaces: a telephone user interface (TUI) and a voice user interface (VUI). You can configure a UM-enabled user's TUI settings when the user accesses a mailbox using the Unified Messaging (UM) system in Exchange Server. When you modify a UM-enabled user's TUI settings on a UM mailbox policy, the changes affect all users who are associated with the UM mailbox policy. You can modify the following TUI settings on a UM mailbox policy:"
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 10960bf0-65cf-4d0b-bae5-d203c53662db
ms.reviewer: 
f1.keywords:
- NOCSH
title: Set mailbox features for Outlook Voice Access users in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Set mailbox features for Outlook Voice Access users in Exchange Online

Outlook Voice Access contains two interfaces: a telephone user interface (TUI) and a voice user interface (VUI). You can configure a UM-enabled user's TUI settings when the user accesses a mailbox using the Unified Messaging (UM) system in Exchange Server. When you modify a UM-enabled user's TUI settings on a UM mailbox policy, the changes affect all users who are associated with the UM mailbox policy. You can modify the following TUI settings on a UM mailbox policy:

- PIN-less access to voice mail

- Voice responses to other messages

- TUI access to their calendar

- TUI access to the directory

- TUI access to their email

- TUI access to their personal contacts

> [!NOTE]
> You can use only Exchange Online PowerShell to modify the Outlook Voice Access TUI settings for UM-enabled users.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](../../voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to modify TUI settings on a UM mailbox policy

This example sets TUI-related settings on a UM mailbox policy named `MyUMMailboxPolicy`.

```PowerShell
Set-UMMailbox -identity MyUMMailboxPolicy -AllowSubscriberAccess $true -AllowTUIAccessToCalendar $false -AllowTUIAccessToDirectory $false -AllowTUIAccessToEmail -$true -AllowTUIAccessToPersonalContacts $true
```
