---
title: 'Release quarantined messages from the spam quarantine mailbox'
TOCTitle: Release quarantined messages from the spam quarantine mailbox
ms:assetid: 7a86bfde-f868-4689-bdec-5f01e52b510d
ms:mtpsurl: https://technet.microsoft.com/library/Aa998920(v=EXCHG.150)
ms:contentKeyID: 49345049
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Release quarantined messages from the spam quarantine mailbox

_**Applies to:** Exchange Server 2013_

You can use Microsoft Outlook to recover a quarantined message from the spam quarantine mailbox.

Spam quarantine is a feature of the Content Filter agent that reduces the risk of losing legitimate messages. When a message meets the spam quarantine threshold, it's wrapped in a non-delivery report (NDR) and delivered to the spam quarantine mailbox. For more information about the spam quarantine feature, see [Spam quarantine](spam-quarantine-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox access" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- This procedure requires that you've configured the anti-spam quarantine mailbox. For more information, see [Configure a spam quarantine mailbox](configure-a-spam-quarantine-mailbox-exchange-2013-help.md).

- To access the quarantine mailbox, you need to configure an Outlook profile for that mailbox and then open the mailbox using Outlook. For more information about configuring and using multiple Outlook profiles, see [Overview of Outlook e-mail profiles](https://support.office.com/article/f544c1ba-3352-4b3b-be0b-8d42a540459d).

- To make it easier to locate the message you want to recover, you can create a custom Outlook form and modify the default view to expose the original sender address in the list of messages. For detailed steps, see [Configure Outlook to show the original sender in the quarantine mailbox](configure-outlook-to-show-the-original-sender-in-the-quarantine-mailbox-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use Outlook 2010 or Outlook 2013 to release a message from the spam quarantine mailbox

1. Open the quarantine mailbox using Outlook 2010 or Outlook 2013 on a client computer.

2. In the **Mail** view, find the message you want to recover in the **Inbox**, and then double-click the message to open it.

3. In the **Move** section of the Ribbon, click **Actions** \> **Resend this Message**.

4. When the message opens, click **Send** to resend the message to the intended recipient.

## Use Outlook 2007 to release a message from the spam quarantine mailbox

1. Open the quarantine mailbox using Outlook 2007 on a client computer.

2. In the **Mail Folders** view, find the message you want to recover in the **Inbox**, and then double-click the message to open it.

3. On the **Report** tab, in the **Respond** group, click **Send Again**.

4. When the message opens, click **Send** to resend the message to the intended recipient.

## How do you know this worked?

To verify that you have successfully released the message from the spam quarantine mailbox, contact the recipient and verify they received the message.
