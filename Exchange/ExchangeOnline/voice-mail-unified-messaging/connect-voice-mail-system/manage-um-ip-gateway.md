---
localization_priority: Normal
description: After you create a Unified Messaging (UM) IP gateway, you can view or configure a variety of settings. For example, you can configure the IP address or a fully qualified domain name (FQDN), configure outgoing call settings, and enable or disable Message Waiting Indicator.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 387e540f-8c59-42d2-a423-99fcf97e00aa
ms.reviewer: 
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Servers.UnifiedMessaging.UMIPGatewayGeneralPropertyPageControl
f1.keywords:
- CSH
title: Manage a UM IP gateway in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage a UM IP gateway in Exchange Online

After you create a Unified Messaging (UM) IP gateway, you can view or configure a variety of settings. For example, you can configure the IP address or a fully qualified domain name (FQDN), configure outgoing call settings, and enable or disable Message Waiting Indicator.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to view or configure UM IP gateway properties

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**. In the list view, select the UM IP gateway that you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. Use the **UM IP Gateway** page to view and configure settings for the UM IP gateway. You can view or configure the following settings:

   - **Status**: This display-only field shows the status of the UM IP gateway.

   - **Name**: Use this box to specify a unique name for the UM IP gateway. This is a display name that appears in the EAC. If you have to change the display name of the UM IP gateway after it's been created, you must first delete the existing UM IP gateway, and then create another UM IP gateway that has the appropriate name. The UM IP gateway name is required, but it's used for display purposes only. Because your organization may use multiple UM IP gateways, we recommend that you use meaningful names for your UM IP gateways. The maximum length of a UM IP gateway name is 64 characters, and it can include spaces.

   - **Address**: You can configure a UM IP gateway with either an IP address or a fully qualified domain name (FQDN). Use this box to specify the IP address or FQDN configured on the VoIP gateway, SIP-enabled PBX, IP PBX, or SBC.

     You can enter alphabetical and numeric characters in this box. IPv4 addresses, IPv6 addresses, and FQDNs are supported. If you use an FQDN, you must also make sure that you have correctly configured a DNS host record for the VoIP gateway so that the host name will be correctly resolved to an IP address. Also, if you use an FQDN instead of an IP address, and the DNS configuration for the UM IP gateway is changed, you must disable and then enable the UM IP gateway to make sure that configuration information for the UM IP gateway is updated correctly.

     If you want to use mutual Transport Layer Security (mutual TLS) between a UM IP gateway and a dial plan operating in either SIP secured or Secured mode, you must configure the UM IP gateway with an FQDN. You must also configure it to listen on port 5061 and verify that any IP gateways or IP PBXs have also been configured to listen for mutual TLS requests on port 5061. To configure a UM IP gateway, run the following command: `Set-UMIPGateway -identity MyUMIPGateway -Port 5061`.

   - **Allow outgoing calls through this UM IP gateway**: Select this check box to allow the UM IP gateway to accept and process outgoing calls. This setting doesn't affect call transfers or incoming calls from a VoIP gateway.

     By default, when the UM IP gateway is created, this setting is enabled. If you disable this setting, users associated with the dial plan won't be able to make outgoing calls through the VoIP gateway, IP PBX, or SBC defined in the **Address** field.

   - **Allow message waiting indicator**: Select this check box to allow voice mail notifications to be sent to users for calls taken by the UM IP gateway. This setting allows the UM IP gateway to receive and send SIP NOTIFY messages for users. This setting is enabled by default and allows message waiting notifications to be sent to users.

     Message Waiting Indicator can refer to any mechanism that indicates the existence of a new or unheard message. The indication that a new voice message has arrived can be found in the Inbox in clients such as Outlook and Outlook on the web (formerly known as Outlook Web App). It can take the form of a Short Messaging Service (SMS) or text message sent to a registered mobile phone, an outbound call made from an Exchange server to a preconfigured number, or a lighted desktop phone lamp for a user.

## Use Exchange Online PowerShell to configure UM IP gateway properties

This example modifies the IP address of a UM IP gateway named `MyUMIPGateway`.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -Address 10.10.10.1
```

This example prevents the UM IP gateway named `MyUMIPGateway` from accepting incoming calls and prevents outgoing calls.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -Address voipgateway.contoso.com -Status 2 -OutcallsAllowed $false
```

This example enables the UM IP gateway to function as a VoIP gateway simulator and can be used with the **Test-UMConnectivity** cmdlet.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -Simulator $true
```

> [!IMPORTANT]
> There is a period of latency before all changes that you make to the configuration of a UM IP gateway replicate to all Exchange servers in the same UM dial plan as the UM IP gateway.

This example prevents the UM IP gateway named `MyUMIPGateway` from accepting incoming calls and prevents outgoing calls, sets an IPv6 address, and allows the UM IP gateway to use IPv4 and IPV6 addresses.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -Address fe80::39bd:88f7:6969:d223%11 -IPAddressFamily Any -Status Disabled -OutcallsAllowed $false
```

## Use Exchange Online PowerShell to view UM IP gateway properties

This example displays a formatted list of all the UM IP gateways in the Active Directory forest.

```PowerShell
Get-UMIPGateway |Format-List
```

This example displays the properties for a UM IP gateway named `MyUMIPGateway`.

```PowerShell
Get-UMIPGateway -Identity MyUMIPGateway
```

This example displays all the UM IP gateways including VoIP gateway simulators in the Active Directory forest.

```PowerShell
Get-UMIPGateway -IncludeSimulator $true
```
