---
localization_priority: Normal
description: Learn about Queue Viewer in Exchange 2016 and Exchange 2019.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: db892f88-5c13-4607-a38c-8845b35ab8b2
ms.reviewer:
title: Queue Viewer
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Queue Viewer

Queue Viewer is part of the Exchange Toolbox that's installed on Mailbox servers and Edge Transport servers in Exchange Server 2016 and Exchange Server 2019. Queue Viewer is a Microsoft Management Console (MMC) snap-in that you can use to view information about and take action on queues and messages in queues. Queue Viewer is useful for troubleshooting mail flow issues and identifying spam.

Queue Viewer is located in the **Mail flow tools** section of the Exchange Toolbox.

To find and open the Exchange Toolbox, use one of the following procedures:

- **Windows 10**: Click **Start** \> **All Apps** \> **Microsoft Exchange Server \<Version\> \>** **Exchange Toolbox**.

- **Windows Server 2012 R2 or Windows 8.1**: On the Start screen, open the Apps view by clicking the down arrow near the lower-left corner or swiping up from the middle of the screen. The **Exchange Toolbox** shortcut is in a group named **Microsoft Exchange Server \<Version\>**.

- **Windows Server 2012**: Use any of the following methods:

  - On the Start screen, click an empty area, and type Exchange Toolbox.

  - On the desktop or the Start screen, press Windows key + Q. In the Search charm, type Exchange Toolbox.

  - On the desktop or the Start screen, move your cursor to the upper-right corner, or swipe left from the right edge of the screen to show the charms. Click the Search charm, and type Exchange Toolbox.

  When the shortcut appears in the results, you can select it.

For more information about queues and messages in queues, see [Queues and messages in queues](queues.md).

## Topics that contain Queue Viewer procedures

The topics in the following table contain procedures that use Queue Viewer:

****

|**Topic**|**Description**|
|:-----|:-----|
|[Connect to a Server in Queue Viewer](https://docs.microsoft.com/exchange/connect-to-a-server-in-queue-viewer-exchange-2013-help)|By default, Queue Viewer opens the queue database on the server where you opened Queue Viewer. However, you can connect to a different server.|
|[Set Queue Viewer Options](https://docs.microsoft.com/exchange/set-queue-viewer-options-exchange-2013-help)|You can configure the queue and message refresh intervals, and the number of items that are displayed on each page.|
|[View queued message properties in Queue Viewer](queued-message-properties.md)|Explains how to use Queue Viewer to view messages, and explains the message properties.|
|[Export Lists from Queue Viewer](https://docs.microsoft.com/exchange/export-lists-from-queue-viewer-exchange-2013-help)|You can use the **Export List** link in the action pane to export the list of queues or a list of messages for troubleshooting and diagnostics.|
|[Queue properties](queue-properties.md)|Describes the queue properties, and shows the properties that are available in Queue View versus the Exchange Management Shell.|
|[Properties of messages in queues](message-properties.md)|Describes the message properties, and shows the properties that are available in Queue View versus the Exchange Management Shell.|
|[Procedures for queues](queue-procedures.md)|Explains how to view, retry, resubmit, suspend, and resume queues.|
|[Procedures for messages in queues](message-procedures.md)|Explains how to remove, suspend, resume, and redirect messages in queues.|
