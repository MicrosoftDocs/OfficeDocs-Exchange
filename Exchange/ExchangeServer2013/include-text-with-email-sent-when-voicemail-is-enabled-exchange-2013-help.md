---
title: 'Include text with the email message sent when a user Is enabled for voice mail: Exchange 2013 Help'
TOCTitle: Include text with the email message sent when a user Is enabled for voice mail
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 3e8292fb-0cdb-445d-8048-a59af7c38d63
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Include text with the email message sent when a user Is enabled for voice mail in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When a user's mailbox is enabled for Unified Messaging (UM) voice mail, an email message is sent that welcomes the user to Unified Messaging. This message contains the PIN information the user will use to first access the voice mail system.

You can customize the text that's sent in the welcome email message by adding text in the **When a user is enabled for Unified Messaging** box on a UM mailbox policy. You can include such information as the UM technical support telephone numbers or additional Outlook Voice Access numbers. After you add the text, it will be included in the email message sent when users associated with the UM mailbox policy are enabled for Unified Messaging.

> [!NOTE]
> The custom text you add to the welcome message is limited to 512 characters, and it can include simple HTML text.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to customize the text sent when a mailbox is enabled for Unified Messaging

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Message text**, in the text box for **When a user is enabled for Unified Messaging**, enter the text you want to include in the email message that's sent when users are enabled for Unified Messaging voice mail.

4. Click **Save**.

## Use the Shell to customize the text sent when a mailbox is enabled for Unified Messaging

This example enables UM-enabled users who are associated with a UM mailbox policy to receive additional instructions about UM and the Outlook Voice Access number that they can use to access their mailbox over a phone.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -UMEnabledText "You've been enabled for Unified Messaging voice mail. To access your Exchange mailbox, call your internal telephone extension number. From outside your office, call 425-555-1234."
```
