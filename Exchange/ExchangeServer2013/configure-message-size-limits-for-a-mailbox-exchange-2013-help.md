---
title: 'Configure message size limits for a mailbox: Exchange 2013 Help'
TOCTitle: Configure message size limits for a mailbox
ms:assetid: d1220685-14c0-4c4f-abb2-3920f3046212
ms:mtpsurl: https://technet.microsoft.com/library/Bb124708(v=EXCHG.150)
ms:contentKeyID: 50383002
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure message size limits for a mailbox

_**Applies to:** Exchange Server 2013_

You can use the EAC and the Shell to configure message size limits for a user mailbox. These limits control the size of messages that a user can send and receive. By default, when a mailbox is created, there isn't a size limit for sent and received messages.

Keep in mind that there are other settings in an Exchange organization that determine the maximum message size a mailbox can send and receive (for example, the maximum message size configured on a Mailbox server). To learn more about the message size restrictions in Exchange, including the types of message size limits, their scope, and the order of precedence, see [Message size limits](message-size-limits-exchange-2013-help.md).

For additional management tasks related to user mailboxes, see [Manage user mailboxes](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-user-mailboxes/manage-user-mailboxes).

> [!NOTE]
> You can also control the size of messages sent and received by mail users and from shared mailboxes.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure message size limits

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to change the message size limits for, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Message Size Restrictions**, click **View details** to view and change the following message size limits:

    - **Sent messages**: To specify a maximum size for messages sent by this user, select the **Maximum message size (KB)** check box and type a value in the box. The message size must be between 0 and 2,097,151 KB. If the user sends a message larger than the specified size, the message will be returned to the user with a descriptive error message.

    - **Received messages**: To specify a maximum size for messages received by this user, select the **Maximum message size (KB)** check box and type a value in the box. The message size must be between 0 and 2,097,151 KB. If the user receives a message larger than the specified size, the message will be returned to the sender with a descriptive error message.

5. Click **OK**, and then click **Save** to save your changes.

## Use the Shell to configure message size limits

This example sets the maximum size for sent messages to 25 MB and the maximum size for received messages to 35 MB for the mailbox of Debra Garcia.

```powershell
Set-Mailbox "Debra Garcia" -MaxSendSize 25mb -MaxReceiveSize 35mb
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).

## How do you know this worked?

To verify that you've successfully configured message size limits for a mailbox, do one of the following:

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to verify the message size limits for, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Message Size Restrictions**, click **View details** to verify the message size limits for the mailbox.

Or

Run the following command in the Shell.

```powershell
Get-Mailbox <identity> | fl MaxSendSize,MaxReceiveSize
```
