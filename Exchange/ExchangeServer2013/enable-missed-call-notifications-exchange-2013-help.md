---
title: 'Enable missed call notifications for a user: Exchange 2013 Help'
TOCTitle: Enable missed call notifications for a user
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: aa0cbb60-5422-474f-af16-621aade31c1f
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable missed call notifications for a user in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable or disable missed call notifications for a Unified Messaging (UM) mailbox policy by using the Shell or the EAC. A missed call notification is an email message that's sent to a user when the user doesn't answer an incoming call and the caller doesn't leave a voice mail message. This is a different email message than the message that contains the voice message that's left for a user.

When you disable missed call notifications on a UM mailbox policy, you prevent all users associated with the UM mailbox policy from receiving an email message when they don't answer an incoming call and the caller doesn't leave a voice message. By default, missed call notifications are enabled for each UM mailbox policy that's created. Also by default, a UM mailbox policy is created every time you create a UM dial plan.

> [!NOTE]
> When you're integrating Unified Messaging and Microsoft Lync Server, missed call notifications aren't available to users that have a mailbox located on an Exchange 2007 or Exchange 2010 Mailbox server when a user disconnects before the call is sent to a Mailbox server running the Microsoft Exchange Unified Messaging service.

For additional management tasks related to UM mailbox policies, see [Manage a UM mailbox policy](manage-um-mailbox-policy-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable missed call notifications for a UM mailbox policy

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **General**, select the check box next to **Allow missed call notifications**.

4. Click **Save**.

## Use the Shell to enable missed call notifications for a UM mailbox policy

This example enables missed call notifications for a UM mailbox policy named `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowMissedCallNotifications $true
```
