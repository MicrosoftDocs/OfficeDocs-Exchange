---
localization_priority: Normal
description: You can retrieve PIN information for a user who is enabled for Unified Messaging (UM). After a user has been enabled for UM-enabled and a PIN is generated or created, the PIN is encrypted and stored in the user's mailbox.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 01517cca-99fe-46b2-b586-19e8d2707728
ms.reviewer: 
f1.keywords:
- NOCSH
title: Retrieve voice mail PIN information in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Retrieve voice mail PIN information in Exchange Online

You can retrieve PIN information for a user who is enabled for Unified Messaging (UM). After a user has been enabled for UM-enabled and a PIN is generated or created, the PIN is encrypted and stored in the user's mailbox.

When you retrieve PIN information for a UM-enabled user, the information returned to you is calculated by using the encrypted PIN data stored in the user's mailbox. This lets you view information from the user's mailbox and also indicates whether the user has been locked out of the mailbox.

For additional tasks related to PIN security, see [PIN security procedures](pin-security-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- Before you perform these procedures, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](../../voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to retrieve PIN information for a UM-enabled user

1. In the EAC, navigate to **Recipients**. In the list view, select the user mailbox that you want to view.

2. In the details pane, under **Phone and Voice Features**, click **View details**.

3. On the **UM Mailbox** page \> **UM mailbox settings**, view the **PIN status** for the user. On this page, you can also reset the voice mail PIN for the user.

## Use Exchange Online PowerShell to retrieve PIN information for a UM-enabled user

This example displays the user ID, whether a PIN is expired, whether the UM mailbox is locked out, and whether Tony is a first-time user.

```PowerShell
Get-UMMailboxPIN -identity tony@contoso.com
```
