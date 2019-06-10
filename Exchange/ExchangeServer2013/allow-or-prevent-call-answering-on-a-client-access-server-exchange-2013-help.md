---
title: 'Allow or prevent call answering on a Client Access server: Exchange 2013 Help'
TOCTitle: Allow or prevent call answering on a Client Access server
ms:assetid: 8287bb78-2621-4b80-a128-8f2ccd67923a
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Bb123529(v=EXCHG.150)
ms:contentKeyID: 49315455
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Allow or prevent call answering on a Client Access server in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can allow the Microsoft Exchange Unified Messaging Call Router service on a Client Access server to answer new calls or prevent it from doing so. By default, a Client Access server is in an enabled state after it's installed. When you're setting the Client Access server to accept incoming voice, fax, auto attendant and Outlook Voice Access calls, you use the **Set-ServerComponentState** cmdlet.

Configuring Maintenance Mode for a Client Access server lets you take the server out of service. For a Client Access server, out-of-service means that the server won't accept any incoming calls from VoIP gateways, IP PBXs, SIP-enabled PBXs, or session border controllers (SBCs).

In Exchange 2007 and Exchange 2010, there was a status parameter that could be used to control the operational status of a Unified Messaging server. In Exchange 2013, no status parameter is available for that purpose on the **Set-UMCallRouterSettings** cmdlet on a Client Access server.

> [!IMPORTANT]
> It's not required that Client Access and Mailbox servers be added to a UM dial plan before they can process calls for Unified Messaging, except when you're integrating UM and Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server. By default, all Client Access and Mailbox servers in an organization are available to answer incoming calls.

For additional management tasks related to Client Access servers, see [UM services procedures](um-services-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange Server Configuration Settings" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Verify that the Client Access server is installed, either on the same computer as the Mailbox server or on a separate computer.

- If you're putting a Client Access server into Maintenance Mode, verify that there's enough healthy capacity in the Client Access array to allow the server to go out of service.

- Before taking a server out of Maintenance Mode, verify the health of the server and make sure it's ready to go into service.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to allow or prevent call answering on a Client Access server

This example enables a Client Access server `UMCallRouter-05x.contoso.com` to answering incoming voice, fax, auto attendant, and Outlook Voice Access calls from VoIP gateways, IP PBXs, SIP-enabled PBXs, and SBCs, and writes the change to the registry on the UMCallRouter-05x server.

```powershell
Set-ServerComponentState -Component UnifiedMessaging -Identity UMCallRouter-05x.contoso.com -Requester Maintenance -State Active -LocalOnly
```

This example prevents a Client Access server `UMCallRouter-05x.contoso.com` from answering incoming voice, fax, auto attendant, and Outlook Voice Access calls from VoIP gateways, IP PBXs, SIP-enabled PBXs, and SBCs, and writes the change only to Active Directory.

```powershell
Set-ServerComponentState -Component UnifiedMessaging -Identity UMCallRouter-05x.contoso.com -Requester Maintenance -State Inactive -RemoteOnly
```
