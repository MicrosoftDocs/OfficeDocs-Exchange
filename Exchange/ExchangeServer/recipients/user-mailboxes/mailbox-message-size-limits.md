---
localization_priority: Normal
description: 'Summary: How to use the Exchange admin center (EAC) and the Exchange Management Shell to set message size limits for a user mailbox.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d1220685-14c0-4c4f-abb2-3920f3046212
ms.reviewer:
title: Configure message size limits for a mailbox
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure message size limits for a mailbox

Message size limits control the size of messages that a user can send and receive. By default, when a mailbox is created, there isn't a size limit for sent and received messages.

Keep in mind that there are other settings in an Exchange organization that determine the maximum message size a mailbox can send and receive (for example, the maximum message size configured on a Mailbox server). To learn more about the message size restrictions in Exchange, including the types of message size limits, their scope, and the order of precedence, see [Message size and recipient limits in Exchange Server](../../mail-flow/message-size-limits.md).

For additional management tasks related to user mailboxes, see [Manage user mailboxes](user-mailboxes.md).

> [!NOTE]
> You can also control the size of messages sent and received by mail users and from shared mailboxes.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to set message size limits

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to change the message size limits for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Message Size Restrictions**, click **View details** to view and change the following message size limits:

   - **Sent messages**: To set a maximum size for messages sent by this user, select the **Maximum message size (KB)** check box and type a value in the box. The message size must be between 0 and 2,097,151 KB. If the user sends a message larger than the specified size, the message will be returned to the user with a descriptive error message.

   - **Received messages**: To set a maximum size for messages received by this user, select the **Maximum message size (KB)** check box and type a value in the box. The message size must be between 0 and 2,097,151 KB. If the user receives a message larger than the specified size, the message will be returned to the sender with a descriptive error message.

5. Click **OK**, and then click **Save** to save your changes.

## Use the Exchange Management Shell to configure message size limits

This example sets the maximum size for sent messages to 25 MB and the maximum size for received messages to 35 MB for the mailbox of Debra Garcia.

```PowerShell
Set-Mailbox -Identity "Debra Garcia" -MaxSendSize 25mb -MaxReceiveSize 35mb
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## How do you know this worked?

To verify that you've successfully set up message size limits for a mailbox, do one of the following:

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to verify the message size limits for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Message Size Restrictions**, click **View details** to verify the message size limits for the mailbox.

   Or

   Run the following command in the Exchange Management Shell.

   ```PowerShell
   Get-Mailbox -Identity <Identity> | Format-List MaxSendSize,MaxReceiveSize
   ```
