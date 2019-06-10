---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 can't continue because the target server's A record is missing from DNS.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.HostRecordMissing
ms.author: chrisda
ms.assetid: 2f18cb65-29fe-4b72-8d68-52fd503d5673
ms.date: 8/2/2018
ms.reviewer: 
title: The Host record for the local computer cannot be found in the DNS database [HostRecordMissing]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# The Host record for the local computer cannot be found in the DNS database [HostRecordMissing]

Exchange Setup can't continue because the Host (A) record for this computer can't be found in the DNS zone for the domain. Setup requires a vaild A record for the server, and Exchange uses email server A records to find the IP address of the next hop to send messages.

To resolve this issue:

- Verify that the local TCP/IP configuration points to the correct DNS server. For more information, see [Configure TCP/IP settings](https://go.microsoft.com/fwlink/p/?linkid=108281).

- Use Nslookup.exe to verify that the Host (A) record exists on the DNS server. For more information, see [To verify A resource records exist in DNS](https://go.microsoft.com/fwlink/?LinkId=63001).

For information about DNS name resolution, troubleshooting, and A records, see the following:

- [Troubleshooting DNS](https://go.microsoft.com/fwlink/p/?LinkId=294828)

- [Managing resource records](https://go.microsoft.com/fwlink/p/?LinkId=294829)

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
