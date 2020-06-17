---
localization_priority: Normal
description: You can allow or prevent voice mail notifications to users for calls received by a Unified Messaging (UM) IP gateway. If you enable this setting, the UM IP gateway can receive and send SIP NOTIFY messages for users. Message Waiting Indicator (MWI) is enabled by default and allows message waiting notifications to be sent to users, but you can turn it off depending on your needs.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5667e37c-48c6-4659-9dc9-94b1dd8ba232
ms.reviewer: 
f1.keywords:
- NOCSH
title: Allow Message Waiting Indicator (MWI) on a UM IP gateway in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Allow Message Waiting Indicator (MWI) on a UM IP gateway in Exchange Online

You can allow or prevent voice mail notifications to users for calls received by a Unified Messaging (UM) IP gateway. If you enable this setting, the UM IP gateway can receive and send SIP NOTIFY messages for users. Message Waiting Indicator (MWI) is enabled by default and allows message waiting notifications to be sent to users, but you can turn it off depending on your needs.

A message waiting indicator notifies a user about a new or unheard voice message. It appears in the Inbox in clients such as Outlook and Outlook on the web (formerly known as Outlook Web App). It can also be a text (SMS) message sent to a registered mobile phone, an outgoing call made from an Exchange server to a number that's been configured for playing new messages, or a lighted lamp on a user's desktop phone.

> [!TIP]
> MWI notifications can also be enabled and disabled on a UM mailbox policy for a group of users.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](../../voice-mail-unified-messaging/connect-voice-mail-system/um-ip-gateway-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-ip-gateway.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to allow Message Waiting Indicator

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM IP Gateway** page, select the check box next to **Allow message waiting indicator**.

3. Click **Save**.

## Use Exchange Online PowerShell to allow Message Waiting Indicator

This example allows the message waiting indicator to appear for users who are associated with the UM IP gateway named `MyUMIPGateway` with an IP address of 10.10.10.1.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -Address 10.10.10.1 -MessageWaitingIndicatorAllowed $true
```
