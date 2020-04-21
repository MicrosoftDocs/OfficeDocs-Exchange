---
title: 'Include text with the email message sent when a fax message is received: Exchange 2013 Help'
TOCTitle: Include text with the email message sent when a fax message is received
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 48244e58-b7d6-4f0e-bbae-d22bf0fc11ff
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Include text with the email message sent when a fax message is received in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can include additional text in the email message that's sent when a fax message is received by a user who is enabled for Unified Messaging (UM) voice mail and is fax-enabled, and when the UM mailbox policy has been configured correctly to use a fax partner provider. By default, the text included when a UM-enabled user receives a fax message indicates only that the user has received a fax message. However, you can create a custom message by adding text in the **When a user receives a fax message** box on a UM mailbox policy. For example, the text can include information about system security policies and describe the correct way to handle fax messages in your organization. After you add the text, it will be included in each email message that's sent when UM-enabled users who are associated with the UM mailbox policy receive a fax message.

> [!NOTE]
> The custom text that accompanies a fax message is limited to 512 characters, and can include simple HTML text.

For additional management tasks related to faxing, see [Faxing procedures](faxing-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging Permissions](https://technet.microsoft.com/library/d326c3bc-8f33-434a-bf02-a83cc26a5498.aspx) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to change the text included with a fax message

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Message text**, in the text box for **When a user receives a fax message**, enter the text you want to include in the email message that's sent when users receive a fax message in their mailbox.

4. Click **Save**.

## Use the Shell to change the text included with a fax message

This example enables UM-enabled users who are associated with a UM mailbox policy to receive additional instructions on how to open a fax message that they've received in their mailbox.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -FaxMessageText "To open this fax message, double-click the file attachment."
```
