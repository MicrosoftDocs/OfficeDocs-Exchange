---
localization_priority: Normal
description: 'Outlook Voice Access contains two interfaces: the telephone user interface (TUI) and the voice user interface (VUI). By default, when users dial in to Outlook Voice Access, they can access their calendar, email, and personal contacts, and search the directory. You can use Exchange Online PowerShell to prevent users from accessing one or more of these features when they use Outlook Voice Access to access their mailbox. When you modify Outlook Voice Access features on a Unified Messaging (UM) mailbox policy, your changes affect all users who are associated with the UM mailbox policy.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 37421edf-af60-4ca9-9e8b-262b8b851607
ms.reviewer: 
f1.keywords:
- NOCSH
title: Disable selected features for Outlook Voice Access users in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Disable selected features for Outlook Voice Access users in Exchange Online

Outlook Voice Access contains two interfaces: the telephone user interface (TUI) and the voice user interface (VUI). By default, when users dial in to Outlook Voice Access, they can access their calendar, email, and personal contacts, and search the directory. You can use Exchange Online PowerShell to prevent users from accessing one or more of these features when they use Outlook Voice Access to access their mailbox. When you modify Outlook Voice Access features on a Unified Messaging (UM) mailbox policy, your changes affect all users who are associated with the UM mailbox policy.

You can disable users' access to the following Outlook Voice Access features on a UM mailbox policy:

- Calendar

- Directory

- Email

- Personal contacts

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](../../voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures.md).

You can also use Exchange Online PowerShell to disable Outlook Voice Access features on the mailbox of a single UM-enabled user. When you do this, the features will be disabled only for that user. Although you can't disable all the Outlook Voice Access features that are found on a UM mailbox policy for a single user, you can disable access to their calendar and to their email.

For additional management tasks related to UM mailboxes, see [Voice mail for users](../../voice-mail-unified-messaging/set-up-voice-mail/voice-mail-for-users.md).

> [!NOTE]
> You can use only Exchange Online PowerShell to modify the Outlook Voice Access features for UM-enabled users on a UM mailbox policy or on the mailbox of a single UM-enabled user.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- Before you perform these procedures, confirm that a user has been enabled for UM. For detailed steps, see [Enable a user for voice mail](../../voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to disable selected Outlook Voice Access features for UM-enabled users on a UM mailbox policy

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

This example prevents users associated with a UM mailbox policy named `MyUMMailboxPolicy` from accessing their calendar when they dial in to Outlook Voice Access.

```PowerShell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -AllowTUIAccessToCalendar $false
```

This example prevents users associated with the UM mailbox policy named `MyUMMailboxPolicy` from accessing the directory when they dial in to Outlook Voice Access.

```PowerShell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -AllowTUIAccessToDirectory $false
```

This example prevents users associated with the UM mailbox policy named `MyUMMailboxPolicy` from accessing their email when they dial in to Outlook Voice Access.

```PowerShell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -AllowTUIAccessToEmail -$false
```

This example prevents users associated with the UM mailbox policy named `MyUMMailboxPolicy` from accessing personal contacts when they dial in to Outlook Voice Access.

```PowerShell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -AllowTUIAccessToPersonalContacts $false
```

## Use Exchange Online PowerShell to disable selected Outlook Voice Access features on the mailbox of a single UM-enabled user

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

This example disables access to the calendar on a UM mailbox named tony@contoso.com when the user dials in to Outlook Voice Access.

```PowerShell
Set-UMMailbox -Identity tony@contoso.com -TUIAccessToCalendarEnabled $false
```

This example disables access to email on a UM mailbox named tony@contoso.com when the user dials in to Outlook Voice Access.

```PowerShell
Set-UMMailbox -Identity tony@contoso.com -TUIAccessToEmailEnabled $false
```
