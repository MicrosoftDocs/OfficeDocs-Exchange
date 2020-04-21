---
title: 'Configure calendar options for POP3: Exchange 2013 Help'
TOCTitle: Configure calendar options for POP3
ms:assetid: ac3d60a0-8697-4c06-9e93-f8d2c4b157b6
ms:mtpsurl: https://technet.microsoft.com/library/Bb124133(v=EXCHG.150)
ms:contentKeyID: 50395403
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure calendar options for POP3

_**Applies to:** Exchange Server 2013_

You can use the Shell to configure calendaring access settings for your users who connect to their mailboxes using POP3 connections. The settings you specify determine how your POP3 users can access their calendar and exchange calendar information (for example, send or respond to a meeting request) with other users.

For additional information related to POP3, see [POP3 and IMAP4 in Exchange Server 2013](pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 settings" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to set the calendar options for POP3

This example enables POP3 users who connect to a server named CAS01 to use the iCalendar standard, a standard for exchanging calendar information.

```powershell
Set-PopSettings -Server CAS01 -CalendarItemRetrievalOption iCalendar
```

This example enables POP3 users to access calendar information from an internal server.

```powershell
Set-PopSettings -Server CAS01 -CalendarItemRetrievalOption IntranetUrl
```

This example enables POP3 users to access calendar information from the Internet on an external server.

```powershell
Set-PopSettings -CalendarItemRetrievalOption InternetUrl
```

This example enables POP3 users to access calendar information by using a direct Outlook Web App URL. If you're using `Custom`, you must specify an Outlook Web App URL using the *OWAServerUrl* parameter.

```powershell
Set-PopSettings -CalendarItemRetrievalOption Custom -OwaServerUrl "https://OwaServer01"
```

After you've specified the calendar options for POP3, you must restart the POP3 services. For information about how to restart the POP3 services, see [Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-PopSettings](https://technet.microsoft.com/library/aa997154\(v=exchg.150\)).

## How do you know this worked?

To verify that you've successfully set calendar options, do the following:

Run the following command in the Shell.

```powershell
Get-PopSettings | format-list
```

Verify that the calendar settings are correct.

## For more information

After you set the calendar options for POP3, you may also want to:

[Configure POP3 and IMAP4 message retrieval format options](configure-pop3-and-imap4-message-retrieval-format-options-exchange-2013-help.md)

[Set connection time-out limits for POP3](set-connection-time-out-limits-for-pop3-exchange-2013-help.md)
