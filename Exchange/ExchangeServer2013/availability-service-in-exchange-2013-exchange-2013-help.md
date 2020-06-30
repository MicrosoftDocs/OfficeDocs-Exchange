---
title: 'Availability service in Exchange 2013: Exchange 2013 Help'
TOCTitle: Availability service in Exchange 2013
ms:assetid: 9722dea2-2bf8-437c-85c0-3ab65b8a07b9
ms:mtpsurl: https://technet.microsoft.com/library/Bb232134(v=EXCHG.150)
ms:contentKeyID: 51492808
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Availability service in Exchange 2013

_**Applies to:** Exchange Server 2013_

The Exchange 2013 Availability service makes free/busy information available to Microsoft Outlook and Outlook Web App clients. The Availability service improves information workers' calendaring and meeting scheduling experience by providing secure, consistent, and up-to-date free/busy information.

Outlook and Outlook Web App use the Availability service to perform the following tasks:

- Retrieve current free/busy information for Exchange 2013 mailboxes

- Retrieve current free/busy information from other Exchange 2013 organizations

- Retrieve published free/busy information from public folders for mailboxes on servers that have previous versions of Exchange

- View attendee working hours

- Show meeting time suggestions

## Overview of the Availability service

The Availability service retrieves free/busy information directly from the target mailbox for users on Exchange 2013, Exchange 2010, or Exchange 2007 and can be configured to retrieve free/busy information for users on earlier versions of Exchange. For topologies that have Exchange 2007, Exchange 2010, or Exchange 2013 mailboxes in which all clients are running Outlook 2007 or higher, the Availability service is used to retrieve free/busy information.

Outlook uses the Exchange Autodiscover service to obtain the URL of the Availability service. For more information about the Autodiscover service, see [Autodiscover service](autodiscover-service-for-exchange-2013.md).

You can use the Exchange Management Shell to configure the Availability service. You can't use the Exchange admin center (EAC) to configure the Availability service.

## Information about away status

The Availability service also provides access to automatic-reply messages that users send when they are out of the office or away for an extended period of time.

Information workers use the Automatic Replies feature (formerly known as Out of Office) in Outlook and Outlook Web App to alert others when they're unavailable to respond to email messages. This functionality makes it easier to set and manage automatic-reply messages for both information workers and administrators.

## Availability service API

The Availability service is part of the Exchange 2013 programming interface. It's available as a web service to let developers write third-party tools for integration purposes.

## Availability service Network Load Balancing

Using Network Load Balancing (NLB) on your Client Access servers that are running the Availability service can improve performance and reliability for your users who rely on free/busy information. Outlook discovers the Availability service URL using the Autodiscover service. To use NLB with the Availability service, you must make changes to your configuration.

The internal URL is used from the intranet, and the external URL is used from the Internet. If you want to use the same URL for both internal and external traffic, make sure that DNS is correctly configured to route internal traffic directly to the internal URL. Also, make sure that the URL can be accessed both internally and externally. For the Autodiscover and Availability services to work, DNS must be configured so that mail.\<*domain name*\>.com and autodiscover.mail.\<*domain name*\>.com point to the virtual IP (VIP) of your load-balancing solution, where \<*domain name*\> is the name of your domain.

> [!NOTE]
> For more information, see <A href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc739506(v=ws.10)">Network Load Balancing Technical Reference</A> and <A href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759510(v=ws.10)">Network Load Balancing Clusters</A>. You can also search for third-party load-balancing software websites.

## Methods used to retrieve free/busy information

The following table lists the different methods used to retrieve free/busy information in different single-forest topologies.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Client</th>
<th>Mailbox retrieving free/busy information is running</th>
<th>Target mailbox is running</th>
<th>Free/busy retrieval method</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook 2013</p></td>
<td><p>Exchange 2013, Exchange 2010, or Exchange 2007</p></td>
<td><p>Exchange 2013, Exchange 2010, or Exchange 2007</p></td>
<td><p>The Availability service reads free/busy information from the target mailbox.</p></td>
</tr>
<tr class="even">
<td><p>Outlook 2007</p></td>
<td><p>Exchange 2013, Exchange 2010, or Exchange 2007</p></td>
<td><p>Exchange 2010 or Exchange 2007</p></td>
<td><p>The Availability service reads free/busy information from the target mailbox.</p></td>
</tr>
<tr class="odd">
<td><p>Outlook 2007</p></td>
<td><p>Exchange 2010 or Exchange 2007</p></td>
<td><p>Exchange 2003</p></td>
<td><p>The Availability service makes HTTP connections to the /public virtual directory of the Exchange 2003 mailbox.</p></td>
</tr>
<tr class="even">
<td><p>Outlook Web App</p></td>
<td><p>Exchange 2013, Exchange 2010, or Exchange 2007</p></td>
<td><p>Exchange 2013, Exchange 2010, or Exchange 2007</p></td>
<td><p>Outlook Web App in Exchange 2010 or Outlook Web Access in Exchange 2007 calls the Availability service API, which reads the free/busy information from the target mailbox.</p></td>
</tr>
</tbody>
</table>
