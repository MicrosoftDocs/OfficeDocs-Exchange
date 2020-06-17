---
localization_priority: Normal
description: When you create a Unified Messaging (UM) IP gateway, you enable Exchange servers to connect to a new Voice over IP (VoIP) gateway, a Private Branch eXchange (PBX) enabled for Session Initiation Protocol (SIP), an IP PBX, or a session border controller (SBC). Immediately after you create a UM IP gateway, you should create a new UM hunt group and then associate the UM hunt group with the UM IP gateway. You can associate the UM IP gateway with one or more UM dial plans by creating one or more UM hunt groups.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 542d6b50-147b-4cec-b54d-61c7b8fc0fc7
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Servers.UnifiedMessaging.CreateUMIPGatewayWizardForm.CreateUMIPGatewayWizardPage
title: Create a UM IP gateway in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create a UM IP gateway in Exchange Online

When you create a Unified Messaging (UM) IP gateway, you enable Exchange servers to connect to a new Voice over IP (VoIP) gateway, a Private Branch eXchange (PBX) enabled for Session Initiation Protocol (SIP), an IP PBX, or a session border controller (SBC). Immediately after you create a UM IP gateway, you should create a new UM hunt group and then associate the UM hunt group with the UM IP gateway. You can associate the UM IP gateway with one or more UM dial plans by creating one or more UM hunt groups.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP gateways**, and then click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

2. On the **New UM IP gateway** page, enter the following information:

   - **Name**: Use this box to specify a unique name for the UM IP gateway. This is a display name that appears in the EAC. If you have to change the display name of the UM IP gateway after it's been created, you must first delete the existing UM IP gateway, and then create another UM IP gateway that has the name that you want. The UM IP gateway name is required, but it's used for display purposes only. Because your organization may use multiple UM IP gateways, we recommend that you use meaningful names for your UM IP gateways. The maximum length of a UM IP gateway name is 64 characters, and it can include spaces. However, it can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

   - **Address**: You can configure a UM IP gateway with either an IP address or a fully qualified domain name (FQDN). Use this box to specify the IP address configured on the VoIP gateway, SIP-enabled PBX, IP PBX, or SBC, or an FQDN. This box accepts only FQDNs that are valid and formatted correctly.

     You can enter alphabetical and numeric characters in this box. IPv4 addresses, IPv6 addresses, and FQDNs are supported. If you want to use mutual Transport Layer Security (mutual TLS) between a UM IP gateway and a dial plan operating in either SIP secured or Secured mode, you must configure the UM IP gateway with an FQDN. You must also configure it to listen on port 5061 and verify that any VoIP gateways or IP PBXs have also been configured to listen for mutual TLS requests on port 5061. To configure a UM IP gateway, run the following command: `Set-UMIPGateway -Identity MyUMIPGateway -Port 5061`.

     If you use an FQDN, you must also make sure that you've correctly configured a DNS host record for the VoIP gateway so that the host name will be correctly resolved to an IP address. Also, if you use an FQDN instead of an IP address, and the DNS configuration for the UM IP gateway is changed, you must disable and then enable the UM IP gateway to make sure that configuration information for the UM IP gateway is updated correctly.

   - **UM dial plan**: Click **Browse** to select the UM dial plan that you want to associate with the UM IP gateway. When you select a UM dial plan to associate with a UM IP gateway, a default UM hunt group is also created and associated with the UM dial plan that you selected. If you don't select a UM dial plan, you must manually create a UM hunt group and then associate that UM hunt group with the UM IP gateway that you create.

3. Click **Save**.

## Use Exchange Online PowerShell to create a UM IP gateway

This example creates a UM IP gateway named `yUMIPGateway` that enables Exchange servers to start accepting calls from a VoIP gateway, a PBX enabled for SIP, an IP PBX, or an SBC that has an IP address of 10.10.10.1.

```PowerShell
New-UMIPGateway -Name MyUMIPGateway -Address 10.10.10.1
```

This example creates a UM IP gateway named `MyUMIPGateway` that enables Exchange servers to start accepting calls from a VoIP gateway, a PBX enabled for SIP, an IP PBX, or an SBC that has an FQDN of MyUMIPGateway.contoso.com and listens on port 5061.

```PowerShell
New-UMIPGateway -Name MyUMIPGateway -Address "MyUMIPGateway.contoso.com" -Port 5061
```

This example creates a UM IP gateway named `yUMIPGateway` and prevents the UM IP gateway from accepting incoming calls or sending outgoing calls, sets an IPv6 address, and allows the UM IP gateway to use IPv4 and IPV6 addresses.

```PowerShell
New-UMIPGateway -Identity MyUMIPGateway -Address fe80::39bd:88f7:6969:d223%11 -IPAddressFamily Any -Status Disabled -OutcallsAllowed $false
```
