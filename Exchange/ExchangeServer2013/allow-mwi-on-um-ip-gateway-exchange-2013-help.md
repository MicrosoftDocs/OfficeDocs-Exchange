---
title: 'Allow Message Waiting Indicator (MWI) on a UM IP gateway: Exchange 2013 Help'
TOCTitle: Allow Message Waiting Indicator (MWI) on a UM IP gateway
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 5667e37c-48c6-4659-9dc9-94b1dd8ba232
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Allow Message Waiting Indicator (MWI) on a UM IP gateway in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can allow or prevent voice mail notifications to users for calls received by a Unified Messaging (UM) IP gateway. If you enable this setting, the UM IP gateway can receive and send SIP NOTIFY messages for users. Message Waiting Indicator (MWI) is enabled by default and allows message waiting notifications to be sent to users, but you can turn it off depending on your needs.

A message waiting indicator notifies a user about a new or unheard voice message. It appears in the Inbox in clients such as Outlook and Outlook Web App. It can also be a text (SMS) message sent to a registered mobile phone, an outgoing call made from an Exchange server to a number that's been configured for playing new messages, or a lighted lamp on a user's desktop phone.

> [!TIP]
> MWI notifications can also be enabled and disabled on a UM mailbox policy for a group of users.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM IP gateways" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to allow Message Waiting Indicator

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM IP Gateway** page, select the check box next to **Allow message waiting indicator**.

3. Click **Save**.

## Use the Shell to allow Message Waiting Indicator

This example allows the message waiting indicator to appear for users who are associated with the UM IP gateway named `MyUMIPGateway` with an IP address of 10.10.10.1.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -Address 10.10.10.1 -MessageWaitingIndicatorAllowed $true
```
