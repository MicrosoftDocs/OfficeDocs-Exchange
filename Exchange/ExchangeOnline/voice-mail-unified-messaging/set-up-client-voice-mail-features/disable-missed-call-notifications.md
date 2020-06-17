---
localization_priority: Normal
description: You can enable or disable missed call notifications for a Unified Messaging (UM) mailbox policy by using Exchange Online PowerShell or the EAC. A missed call notification is an email message that's sent to a user when the user doesn't answer an incoming call and the caller doesn't leave a voice message. This is a different email message than the one that contains the voice message that's left for a user.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: e54937d5-3074-454f-b561-e601fecfc6ad
ms.reviewer: 
f1.keywords:
- NOCSH
title: Disable missed call notifications for a user in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Disable missed call notifications for a user in Exchange Online

You can enable or disable missed call notifications for a Unified Messaging (UM) mailbox policy by using Exchange Online PowerShell or the EAC. A missed call notification is an email message that's sent to a user when the user doesn't answer an incoming call and the caller doesn't leave a voice message. This is a different email message than the one that contains the voice message that's left for a user.

When you disable missed call notifications on a UM mailbox policy, you prevent all users associated with the UM mailbox policy from receiving an email message when they don't answer an incoming call and the caller doesn't leave a voice message. By default, missed call notifications are enabled for each UM mailbox policy that's created. Also by default, a UM mailbox policy is created every time you create a UM dial plan.

> [!NOTE]
> When you're integrating Unified Messaging and Microsoft Lync Server, missed call notifications aren't available to users that have a mailbox located on an Exchange 2007 or Exchange 2010 Mailbox server when a user disconnects before the call is sent to a Mailbox server running the Microsoft Exchange Unified Messaging service.

For additional management tasks related to UM mailbox policies, see [Manage a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to disable missed call notifications for a UM mailbox policy

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **General**, clear the check box next to **Allow missed call notifications**.

4. Click **Save**.

## Use Exchange Online PowerShell to disable missed call notifications for a UM mailbox policy

This example disables missed call notifications for a UM mailbox policy named `MyUMMailboxPolicy`.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowMissedCallNotifications $false
```
