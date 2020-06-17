---
localization_priority: Normal
description: By default, when a Unified Messaging (UM) IP gateway is created, its status is set to enabled. However, you might need to disable the UM IP gateway to take it offline and not allow it to take incoming or outgoing calls. After you create a UM IP gateway, you can control its operation and functionality by setting its status variable to enabled or disabled.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 2706ae06-c45d-41b7-abbe-378a9fca104a
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable a UM IP gateway in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable a UM IP gateway in Exchange Online

By default, when a Unified Messaging (UM) IP gateway is created, its status is set to enabled. However, you might need to disable the UM IP gateway to take it offline and not allow it to take incoming or outgoing calls. After you create a UM IP gateway, you can control its operation and functionality by setting its status variable to enabled or disabled.

 For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created and has been disabled. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable a UM IP gateway

1. In the EAC, navigate to \> **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway you want to enable, and then click the **Up arrow** ![Up Arrow Icon](../../media/ITPro_EAC_UpArrowIcon.gif).

2. On the **Warning** page, click **Yes**.

## Use Exchange Online PowerShell to enable a UM IP gateway

This example enables a UM IP gateway named `MyUMIPGateway`.

```PowerShell
Enable-UMIPGateway -Identity MyUMIPGateway
```
