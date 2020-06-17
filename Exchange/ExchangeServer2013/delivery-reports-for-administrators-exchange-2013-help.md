---
title: 'Delivery reports for administrators: Exchange 2013 Help'
TOCTitle: Delivery reports for administrators
ms:assetid: d98623d3-e0b7-4cb9-93fb-6351b4a06137
ms:mtpsurl: https://technet.microsoft.com/library/JJ919241(v=EXCHG.150)
ms:contentKeyID: 50646520
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Delivery reports for administrators

_**Applies to:** Exchange Server 2013_

With delivery reports for administrators, you can track delivery information about messages sent by or received from any specific mailbox in your organization. Specifically, delivery reports for administrators uses the Exchange admin center (EAC) to perform a targeted search of the message tracking logs. The search is always scoped to a specific mailbox. You can search for messages sent by the mailbox, or sent to the mailbox, and you can filter the search results by the message subject.

The content of the message body isn't returned in a delivery report, but the subject line is displayed in the results. If you want to search the mailboxes in your organization for specific email messages based on message content, see [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery).

You may find delivery report searches useful in the following situations:

  - A manager gives a poor review for a trainee because the trainee didn't turn in an assignment on time. The trainee insists he sent a message with the assignment attached. The manager asks you to verify the status of the message.

  - A security bulletin has been sent to users asking that they reply immediately, but no one has replied. Are they ignoring the message or did they just not receive it?

  - Users complain that no one is receiving their messages. They check delivery status for their mail but can't figure out what is going on. This may be because a rule is being applied to messages at the organization level.

After you create a delivery report search, the resulting delivery report will show the following information: Who the message was sent from and to, the subject line, and when the message was sent. The delivery report also shows message delivery status and reasons why delivery may be delayed or failed.

## More about delivery reports

- Here's how to create a new delivery report: [Track messages with delivery reports](track-messages-with-delivery-reports-exchange-2013-help.md).

- On-premises Exchange organizations can use the Exchange Management Shell to query the message tracking logs directly. For more information, see [Search message tracking logs](search-message-tracking-logs-exchange-2013-help.md).

- If your organization contains a previous version of Exchange, you need to consider how the delivery reports feature in Exchange 2013 works across Exchange versions.

  - Exchange 2013 delivery reports can track messages across Exchange 2010 servers in the same Active Directory site.

  - Exchange 2013 delivery reports can't track messages across Exchange 2007 servers in the same Active Directory site. The delivery reports feature uses a remote procedure call and a web service interface that doesn't exist in Exchange 2007.
