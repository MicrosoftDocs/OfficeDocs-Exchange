---
title: 'View anti-spam stamps in Outlook: Exchange 2013 Help'
TOCTitle: View anti-spam stamps in Outlook
ms:assetid: cddb5dbf-ad1e-471c-9fc8-28ddcf7ec1d0
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Bb124595(v=EXCHG.150)
ms:contentKeyID: 49345061
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# View anti-spam stamps in Outlook

_**Applies to:** Exchange Server 2013_

You can use Microsoft Outlook to view the anti-spam stamps that Microsoft Exchange applied to an email message. Anti-spam stamps help you diagnose spam-related problems by applying diagnostic metadata, or stamps, such as sender-specific information, puzzle validation results, and content filtering results to messages as the messages pass through the anti-spam agents that filter inbound messages from the Internet.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox access" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use Outlook 2010 or Outlook 2013 to view anti-spam stamps

1. In Outlook 2010 or Outlook 2013, on a client computer, in the **Mail** view, double-click a message to open it.

2. In the **Tags** section of the Ribbon, click the **Options** icon to display the message **Properties** dialog box.

3. In the **Properties** dialog box, in the **Internet headers** section, use the scroll bar to view the anti-spam stamps as shown in the following example.

   ```powershell
   X-MS-Exchange-Organization-PCL:7
   X-MS-Exchange-Organization-SCL:6
   X-MS-Exchange-Organization-Antispam-Report: DV:3.1.3924.1409;SID:SenderIDStatus Fail;PCL:PhishingLevel SUSPICIOUS;CW:CustomList;PP:Presolved;TIME:TimeBasedFeatures
   ```

## Use Outlook 2007 to view anti-spam stamps

1. In Outlook 2007, on a client computer, in the **Mail** view, double-click a message to open it.

2. On the **Message** tab, in the **Options** group, click **Message Options**.

3. In the **Message Options** dialog box, in the **Internet headers** section, use the scroll bar to view the anti-spam stamps as shown in the following example.

   ```powershell
   X-MS-Exchange-Organization-PCL:7
   X-MS-Exchange-Organization-SCL:6
   X-MS-Exchange-Organization-Antispam-Report: DV:3.1.3924.1409;SID:SenderIDStatus Fail;PCL:PhishingLevel SUSPICIOUS;CW:CustomList;PP:Presolved;TIME:TimeBasedFeatures
   ```
