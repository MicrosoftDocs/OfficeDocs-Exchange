---
localization_priority: Normal
description: Before you create a Unified Messaging (UM) IP gateway, you must first set the IP address or the fully qualified domain name (FQDN) on the VoIP gateway, IP PBX, or session border controller (SBC) that you're using. Then, when you create the UM IP gateway, you set the IP address or FQDN. You can change the IP address or FQDN later.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 100541c1-2297-4c46-9602-b304736541a8
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure the IP address in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the IP address in Exchange Online

Before you create a Unified Messaging (UM) IP gateway, you must first set the IP address or the fully qualified domain name (FQDN) on the VoIP gateway, IP PBX, or session border controller (SBC) that you're using. Then, when you create the UM IP gateway, you set the IP address or FQDN. You can change the IP address or FQDN later.

You can configure the IP address or FQDN using either the EAC or Exchange Online PowerShell. In the EAC, the **Address** box on the **UM IP gateway** page can accept an IPv4 IP address, an IPv6 address, or an FQDN. You can also use the _Address_ parameter on the **Set-UMIPGateway** cmdlet in Exchange Online PowerShell to set an IPv4 IP address, an IPv6 address, or an FQDN. If you create a UM IP gateway using an FQDN, you must create the appropriate HOST A records in your DNS forward lookup zone. If the DNS configuration for the UM IP gateway is changed, you must disable and then enable the UM IP gateway to make sure that its configuration information is updated correctly.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure the IP address on a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway that you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM IP gateway** page, in the **Address** box, enter the IP address for the VoIP gateway, IP PBX, or session border controller (SBC).

3. Click **Save** to save your changes.

> [!IMPORTANT]
> If you use an FQDN instead of an IP address on the UM IP gateway, verify that the correct DNS records have been created.

## Use Exchange Online PowerShell to configure the IP address on a UM IP gateway

This example configures a UM IP gateway named `MyUMIPGateway` with an IP address of 10.10.10.1.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -Address 10.10.10.1
```

This example configures a UM IP gateway named `MyUMIPGateway` with an IP address of 10.10.10.10 and listens for SIP requests on TCP port 5061.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -Address 10.10.10.10 -Port 5061
```

This example prevents the UM IP gateway named `MyUMIPGateway` from accepting incoming and outgoing calls, sets an IPv6 address, and allows the UM IP gateway to use IPv4 and IPV6 addresses.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -Address fe80::39bd:88f7:6969:d223%11 -IPAddressFamily Any -Status Disabled -OutcallsAllowed $false
```
