---
title: 'Direct Push: Exchange 2013 Help'
TOCTitle: Direct Push
ms:assetid: 373c1629-3d4b-4828-b014-9e103de4ef25
ms:mtpsurl: https://technet.microsoft.com/library/Aa997252(v=EXCHG.150)
ms:contentKeyID: 48384981
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Direct Push

_**Applies to:** Exchange Server 2013_

Direct Push is a feature that's built into Microsoft Exchange Server 2013. Direct Push keeps a mobile device current over a cellular or wireless network connection. It notifies the mobile device when new content is ready to be synchronized.

## Overview

For Direct Push to work, the mobile device must be Direct Push capable. These devices include mobile phones that are produced by Microsoft Exchange ActiveSync licensees and that are designed specifically to be Direct Push compatible

By default, Direct Push is enabled in Exchange 2013. Mobile devices that support Direct Push issue a long-lived HTTPS request to the server running Microsoft Exchange. The Exchange server monitors activity on the user's mailbox and sends a response to the mobile device if there are any changes, such as new or changed email, calendar, contact, or task items. If changes occur within the lifespan of the HTTPS request, the Exchange server issues a response to the device that states that changes have occurred and the device should initiate synchronization with the Exchange server. The device then issues this request to the server. When synchronization is complete, a new long-lived HTTPS request is generated to start the process again. This guarantees that email, calendar, contact, and task items are delivered quickly to the mobile device, and that it is always synchronized with the Exchange server.

## Direct Push topology

Direct Push operates in the following way:

1. A mobile device that's configured to synchronize with an Exchange 2013 server issues an HTTPS request to the server. This request is known as a PING. The request tells the server to notify the device if any items change in the next 15 minutes in any folder that's configured to synchronize. Otherwise, the server should return an HTTP 200 OK message. The mobile device then stands by. The 15-minute time span is known as a *heartbeat interval*.

2. If no items change in 15 minutes, the server returns a response of HTTP 200 OK. The mobile device receives this response, resumes activity (known as *waking up*), and issues its request again. This restarts the process.

3. If any items change or new items are received within the 15-minute heartbeat interval, the server sends a response that informs the mobile device that there's a new or changed item and provides the name of the folder in which the new or changed item resides. After the mobile device receives this response, it issues a synchronization request for the folder that has the new or changed item. When synchronization is complete, the mobile device issues a new PING request and the whole process starts over.

Direct Push depends on network conditions that support a long-standing HTTPS request. If the carrier network for the mobile device or the firewall doesn't support long-standing HTTPS requests, the HTTPS request is stopped. The following steps describe how Direct Push operates when the carrier network for a mobile device has a time-out value of 13 minutes.

1. A mobile device issues an HTTPS request to the server. The request tells the server to notify the device if any items change in the next 15 minutes in any folder that's configured to synchronize. Otherwise, the server should return an HTTP 200 OK message. The mobile device then stands by.

2. If the server doesn't respond after 15 minutes, the mobile device wakes up and concludes that the connection to the server was timed out by the network. The device reissues the HTTPS request, but this time it uses a heartbeat interval of 8 minutes.

3. After 8 minutes, the server sends an HTTP 200 OK message. The device then tries to gain a longer connection by issuing a new HTTPS request to the server that has a heartbeat interval of 12 minutes.

4. After 4 minutes, a new email message is received and the server responds by sending an HTTPS request that tells the device to synchronize. The device synchronizes and reissues the HTTPS request that has a heartbeat of 12 minutes.

5. After 12 minutes, if there are no new or changed items, the server responds by sending an HTTP 200 OK message. The device wakes up and concludes that network conditions support a heartbeat interval of 12 minutes. The device then tries to gain a longer connection by reissuing an HTTPS request that has a heartbeat interval of 16 minutes.

6. After 16 minutes, no response is received from the server. The device wakes up and concludes that network conditions cannot support a heartbeat interval of 16 minutes. Because this failure occurred directly after the device tried to increase the heartbeat interval, it concludes that the heartbeat interval has reached its maximum limit. The device then issues an HTTPS request that has a heartbeat interval of 12 minutes because this was the last successful heartbeat interval.

The mobile device tries to use the longest heartbeat interval the network supports. This extends battery life on the device and reduces how much data is transferred over the network. Mobile carriers can specify a maximum, minimum, and initial heartbeat value in the registry settings for the mobile device.

## Configuring Direct Push to work through your firewall

For Direct Push to work through your firewall, you must open TCP port 443. This port is required for Secure Sockets Layer (SSL) and must be opened between the Internet and the Client Access server.

In addition to opening ports on your firewall, for optimal Direct Push performance, you should increase the time-out value on your firewall from the default of 15 minutes to 30 minutes. The maximum length of the HTTPS request is determined by the following settings:

- The maximum time-out value that's set on the firewalls that control the traffic from the Internet to the Client Access server

- The firewall time-out values that are set by the mobile service provider
