---
title: 'Enable outgoing calls on UM IP gateways: Exchange 2013 Help'
TOCTitle: Enable outgoing calls on UM IP gateways
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c3ad8e53-d37e-499e-b1f1-defb0ba1bd12
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable outgoing calls on UM IP gateways in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable outgoing calls for a Unified Messaging (UM) IP gateway if outgoing calls have been disabled. When you select the **Allow outgoing calls through this UM IP gateway** option on the properties for the UM IP gateway, you configure the UM IP gateway to accept and send outgoing calls to a Voice over IP (VoIP) gateway, Private Branch eXchange (PBX) enabled for Session Initiation Protocol (SIP), IP PBX, or session border controller (SBC). Although the **Allow outgoing calls through this UM IP gateway** setting controls whether the UM IP gateway is able to initiate outgoing calls for users, it doesn't affect call transfers or incoming calls from a VoIP gateway, PBX enabled for SIP, IP PBX, or SBC.

Outdialing is the term used to describe a situation in which a user in one UM dial plan initiates a call to a UM-enabled user in another dial plan or to an external telephone number.

To allow outdialing for UM-enabled users, you must:

- Verify that the UM IP gateway allows outgoing calls.

- Create dialing rule groups by creating dialing rule entries on the UM dial plan associated with the UM IP gateway.

- Add the correct dialing rule groups to the list of dialing restrictions in **Dialing authorization** on the UM dial plan, auto attendant, or UM mailbox policy.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM IP gateways" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable outgoing calls for a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM IP Gateway** page, select the check box next to **Allow outgoing calls through this UM IP gateway**.

3. Click **Save**.

## Use the Shell to enable outgoing calls for a UM IP gateway

This example enables outgoing calls on a UM IP gateway named `MyUMIPGateway`.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -OutcallsAllowed $true
```
