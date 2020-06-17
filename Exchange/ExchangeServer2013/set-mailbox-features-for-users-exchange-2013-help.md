---
title: 'Set mailbox features for Outlook Voice Access users: Exchange 2013 Help'
TOCTitle: Set mailbox features for Outlook Voice Access users
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 10960bf0-65cf-4d0b-bae5-d203c53662db
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set mailbox features for Outlook Voice Access users in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Outlook Voice Access contains two interfaces: a telephone user interface (TUI) and a voice user interface (VUI). You can configure a UM-enabled user's TUI settings when the user accesses a mailbox using the Unified Messaging (UM) system in Exchange 2013. When you modify a UM-enabled user's TUI settings on a UM mailbox policy, the changes affect all users who are associated with the UM mailbox policy. You can modify the following TUI settings on a UM mailbox policy:

- PIN-less access to voice mail

- Voice responses to other messages

- TUI access to their calendar

- TUI access to the directory

- TUI access to their email

- TUI access to their personal contacts

> [!NOTE]
> You can use only the Shell to modify the Outlook Voice Access TUI settings for UM-enabled users.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to modify TUI settings on a UM mailbox policy

This example sets TUI-related settings on a UM mailbox policy named `MyUMMailboxPolicy`.

```powershell
Set-UMMailbox -identity MyUMMailboxPolicy -AllowSubscriberAccess $true -AllowTUIAccessToCalendar $false -AllowTUIAccessToDirectory $false -AllowTUIAccessToEmail -$true -AllowTUIAccessToPersonalContacts $true
```
