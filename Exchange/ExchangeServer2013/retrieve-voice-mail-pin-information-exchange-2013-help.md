---
title: 'Retrieve voice mail PIN information: Exchange 2013 Help'
TOCTitle: Retrieve voice mail PIN information
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 01517cca-99fe-46b2-b586-19e8d2707728
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Retrieve voice mail PIN information in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can retrieve PIN information for a user who is enabled for Unified Messaging (UM). After a user has been enabled for UM-enabled and a PIN is generated or created, the PIN is encrypted and stored in the user's mailbox.

When you retrieve PIN information for a UM-enabled user, the information returned to you is calculated by using the encrypted PIN data stored in the user's mailbox. This lets you view information from the user's mailbox and also indicates whether the user has been locked out of the mailbox.

For additional tasks related to PIN security, see [PIN security procedures](pin-security-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform these procedures, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to retrieve PIN information for a UM-enabled user

1. In the EAC, navigate to **Recipients**. In the list view, select the user mailbox that you want to view.

2. In the details pane, under **Phone and Voice Features**, click **View details**.

3. On the **UM Mailbox** page \> **UM mailbox settings**, view the **PIN status** for the user. On this page, you can also reset the voice mail PIN for the user.

## Use the Shell to retrieve PIN information for a UM-enabled user

This example displays the user ID, whether a PIN is expired, whether the UM mailbox is locked out, and whether Tony is a first-time user.

```powershell
Get-UMMailboxPIN -identity tony@contoso.com
```
