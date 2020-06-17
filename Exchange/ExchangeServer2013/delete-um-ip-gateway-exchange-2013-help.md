---
title: 'Delete a UM IP gateway: Exchange 2013 Help'
TOCTitle: Delete a UM IP gateway
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 569d3741-67dd-4597-8d28-010011be0c12
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Delete a UM IP gateway in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you delete a Unified Messaging (UM) IP gateway, Exchange servers can no longer accept incoming calls from the Voice over IP (VoIP) gateway, Session Initiation Protocol (SIP)-enabled Private Branch eXchange (PBX), IP PBX, or session border controller (SBC) associated with the UM IP gateway.

> [!IMPORTANT]
> You should delete a UM IP gateway only when you fully understand the implications of disabling communication with a VoIP gateway, IP PBX, or SBC.

For additional tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM IP gateways" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to delete a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway you want to delete, and then click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif).

2. On the **Warning** page, click **Yes**.

## Use the Shell to delete a UM IP gateway

This example deletes the UM IP gateway named `MyUMIPGateway`.

```powershell
Remove-UMIPGateway -Identity MyUMIPGateway
```
