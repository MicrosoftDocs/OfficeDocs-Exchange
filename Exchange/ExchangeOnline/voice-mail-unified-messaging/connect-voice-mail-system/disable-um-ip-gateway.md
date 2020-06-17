---
localization_priority: Normal
description: By default, when you create a Unified Messaging (UM) IP gateway, the status of the UM IP gateway is enabled. After the UM IP gateway is created, you can disable the operation of the gateway by setting its status to disabled. After you disable the UM IP gateway, the Voice over IP (VoIP) gateway, IP Private Branch eXchange (PBX), or session border controller (SBC) that it's configured to use can no longer process incoming Unified Messaging calls.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: fe3a8797-1230-49cb-a839-ccec238266b6
ms.reviewer: 
f1.keywords:
- NOCSH
title: Disable a UM IP gateway in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Disable a UM IP gateway in Exchange Online

By default, when you create a Unified Messaging (UM) IP gateway, the status of the UM IP gateway is enabled. After the UM IP gateway is created, you can disable the operation of the gateway by setting its status to disabled. After you disable the UM IP gateway, the Voice over IP (VoIP) gateway, IP Private Branch eXchange (PBX), or session border controller (SBC) that it's configured to use can no longer process incoming Unified Messaging calls.

 For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created and is enabled. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway.md) and [Enable a UM IP gateway](enable-um-ip-gateway.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to disable a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway you want to disable, and then click the **Down arrow** ![Down Arrow Icon](../../media/ITPro_EAC_DownArrowIcon.gif).

2. On the **Warning** page, click **Yes**.

## Use Exchange Online PowerShell to disable a UM IP gateway

This example disables a UM IP gateway named `yUMIPGateway` and stops it from accepting incoming calls from a VoIP gateway, IP PBX, or SBC.

```PowerShell
Disable-UMIPGateway -Identity MyUMIPGateway
```

This example disables a UM IP gateway named `yUMIPGateway` and disconnects all current calls immediately.

```PowerShell
Disable-UMIPGateway -Identity MyUMIPGateway -Immediate $true
```
