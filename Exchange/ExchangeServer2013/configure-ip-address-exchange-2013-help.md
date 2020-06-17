---
title: 'Configure the IP address: Exchange 2013 Help'
TOCTitle: Configure the IP address
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 100541c1-2297-4c46-9602-b304736541a8
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the IP address in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Before you create a Unified Messaging (UM) IP gateway, you must first set the IP address or the fully qualified domain name (FQDN) on the VoIP gateway, IP PBX, or session border controller (SBC) that you're using. Then, when you create the UM IP gateway, you set the IP address or FQDN. You can change the IP address or FQDN later.

You can configure the IP address or FQDN using either the EAC or the Shell. In the EAC, the **Address** box on the **UM IP gateway** page can accept an IPv4 IP address, an IPv6 address, or an FQDN. You can also use the _Address_ parameter on the **Set-UMIPGateway** cmdlet in the Shell to set an IPv4 IP address, an IPv6 address, or an FQDN. If you create a UM IP gateway using an FQDN, you must create the appropriate HOST A records in your DNS forward lookup zone. If the DNS configuration for the UM IP gateway is changed, you must disable and then enable the UM IP gateway to make sure that its configuration information is updated correctly.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM IP gateways" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure the IP address on a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway that you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM IP gateway** page, in the **Address** box, enter the IP address for the VoIP gateway, IP PBX, or session border controller (SBC).

3. Click **Save** to save your changes.

> [!IMPORTANT]
> If you use an FQDN instead of an IP address on the UM IP gateway, verify that the correct DNS records have been created.

## Use the Shell to configure the IP address on a UM IP gateway

This example configures a UM IP gateway named `MyUMIPGateway` with an IP address of 10.10.10.1.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -Address 10.10.10.1
```

This example configures a UM IP gateway named `MyUMIPGateway` with an IP address of 10.10.10.10 and listens for SIP requests on TCP port 5061.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -Address 10.10.10.10 -Port 5061
```

This example prevents the UM IP gateway named `MyUMIPGateway` from accepting incoming and outgoing calls, sets an IPv6 address, and allows the UM IP gateway to use IPv4 and IPV6 addresses.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -Address fe80::39bd:88f7:6969:d223%11 -IPAddressFamily Any -Status Disabled -OutcallsAllowed $false
```
