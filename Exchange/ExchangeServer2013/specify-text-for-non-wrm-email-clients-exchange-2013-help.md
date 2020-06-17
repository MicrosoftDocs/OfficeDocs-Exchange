---
title: 'Specify the text to display for email clients that do not support Windows Rights Management: Exchange 2013 Help'
TOCTitle: Specify the text to display for email clients that don't support Windows Rights Management
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: a9b2238a-b534-469c-a0c3-2768bc3d005b
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Specify the text to display for email clients that do not support Windows Rights Management in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can specify the text that will be sent to a user when they receive a protected voice message but their email client doesn't support Information Rights Management (IRM) or Windows Rights Management.

Protected Voice Mail can be accessed only by email clients that support Windows Rights Management or when a UM-enabled user uses Outlook Voice Access to access a protected voice message.

Protected Voice Mail is encrypted. When a voice message is protected:

- The message is marked as Private in Microsoft Outlook and Outlook Web App.

- The voice message can be opened only by the intended recipient of the voice message.

- The recipient can reply to the voice message, but can't forward it to someone who wasn't included on the original voice message.

If a protected voice message is sent to someone whose email client doesn't support Windows Rights Management and isn't accessing the message using Outlook Voice Access, an email message will be sent to them that includes the text you specify. This text should include instructions about what the called party should do to be able to receive the protected voice message.

For additional management tasks related to Protected Voice Mail procedures, see [Protected Voice Mail procedures](protected-voice-mail-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use EAC to specify the text to display for email clients that don't support Windows Rights Management

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Protected voice mail**, under **Message to send to users who don't have Windows Rights Management support**, type the message text in the text box.

4. Click **Save**.

## Use the Shell to specify the text to display for email clients that don't support Windows Rights Management

This example specifies the text to display to users associated with the UM mailbox policy named `MyUMMailboxPolicy` who have email clients that don't support Windows Rights Management.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -ProtectedVoiceMailText "Your email client software does not support Protected Voice Mail. Please contact the Help Desk."
```
