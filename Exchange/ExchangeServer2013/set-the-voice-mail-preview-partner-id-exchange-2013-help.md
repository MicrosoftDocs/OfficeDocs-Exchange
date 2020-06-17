---
title: 'Set the Voice Mail Preview partner ID: Exchange 2013 Help'
TOCTitle: Set the Voice Mail Preview partner ID
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: ab98c320-9952-47a7-b141-ddfc2c0ad419
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set the Voice Mail Preview partner ID in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can set a Voice Mail Preview partner ID on a Unified Messaging (UM) mailbox policy. After you've set the Voice Mail Preview partner ID on a UM mailbox policy, the setting will apply to all UM-enabled users who are linked with that mailbox policy.

> [!NOTE]
> You must use the Shell to set the Voice Mail Preview partner ID.

For more information about the Voice Mail Preview partner program, see [Voice Mail Preview advisor](voice-mail-preview-advisor-exchange-2013-help.md).

For additional management tasks related to voice mail preview, see [Voice Mail Preview procedures](voice-mail-preview-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to set the Voice Mail Preview partner ID on a UM mailbox policy

This example sets the Voice Mail Preview partner ID to CON123-2010 on a UM mailbox policy named _MyUMMailboxPolicy_.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy
-VoiceMailPreviewPartnerAssignedID CON123-2010
```
