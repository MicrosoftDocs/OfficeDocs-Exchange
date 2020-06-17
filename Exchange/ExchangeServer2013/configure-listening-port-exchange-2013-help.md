---
title: 'Configure the listening port: Exchange 2013 Help'
TOCTitle: Configure the listening port
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 200ecbd8-18c3-4594-9cc8-924b3ab4eca1
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the listening port in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can configure the TCP port that's used to listen for Session Initiation Protocol (SIP) requests on a Unified Messaging (UM) IP gateway. By default, when you create a UM IP gateway, the TCP SIP listening port number is set to 5060. The TCP SIP listening port can't be configured or changed by using the EAC. You must configure the TCP SIP listening port number by using the **Set-UMIPGateway** cmdlet.

You may have to configure the TCP listening port number to 5061 if you want to:

- Set the VoIP security setting on a UM dial plan to SIP Secured.

- Set the VoIP security setting on a UM dial plan to Secured.

- Integrate with Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server.

- Use mutual Transport Layer Security (mutual TLS) to encrypt network data between Exchange servers and a VoIP gateway, Private Branch eXchange (PBX) enabled for SIP, IP PBX, or session border controller (SBC).

If you want to use mutual TLS between a UM IP gateway and a dial plan operating in either SIP Secured or Secured mode, when you create the UM IP gateway you must configure it with a fully qualified domain name (FQDN) and then use the Shell to configure the UM IP gateway to listen on TCP port 5061. You must also verify that any VoIP gateways, PBXs enabled for SIP, IP PBXs, and SBCs have also been configured to listen for mutual TLS requests on port 5061.

> [!IMPORTANT]
> When you create a UM IP gateway using an FQDN, you must create the appropriate HOST (A) records in your DNS forward lookup zone. If you create a UM IP gateway using an FQDN, and the DNS configuration for the UM IP gateway is changed, you must disable and then enable the UM IP gateway to make sure that the UM IP gateway's configuration information is updated correctly.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM IP gateways" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform this procedure, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to configure the TCP listening port

This example configures a UM IP gateway named `MyUMIPGateway` that has an FQDN of mTLS.MyUMIPGateway.contoso.com and listens for SIP requests on TCP port 5061.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -Address mTLS.MYUMIPGateway.contoso.com -Port 5061
```

This example configures a UM IP gateway named `MyUMIPGateway` that has an FQDN of SIPSecured.MyUMIPGateway.contoso.com and listens for SIP requests on TCP port 5061.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -Address SIPSecured.MyUMIPGateway.contoso.com -Port 5061
```

This example configures a UM IP gateway named `MyUMIPGateway` that has an FQDN of MyOCSUMIPGateway.contoso.com and listens for SIP requests on TCP port 5061.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -Address MyOCSUMIPGateway.contoso.com -Port 5061
```
