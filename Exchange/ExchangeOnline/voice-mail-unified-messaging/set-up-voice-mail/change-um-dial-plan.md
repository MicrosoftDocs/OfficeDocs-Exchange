---
localization_priority: Normal
description: You may need to move a user who is enabled for Unified Messaging (UM) to a different UM dial plan or change the dial plan that's associated with the user. For example, you might want to move a UM-enabled user from a Telephone Extension dial plan to a SIP URI dial plan.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4a6b6b6f-c61c-44e8-91dd-c5d28835f441
ms.reviewer: 
f1.keywords:
- NOCSH
title: Change the UM dial plan in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Change the UM dial plan in Exchange Online

You may need to move a user who is enabled for Unified Messaging (UM) to a different UM dial plan or change the dial plan that's associated with the user. For example, you might want to move a UM-enabled user from a Telephone Extension dial plan to a SIP URI dial plan.

To change the UM dial plan, you'll have to disable the user for Unified Messaging and then enable the user for Unified Messaging on the new UM dial plan. This is because different dial plans may have different settings and requirements, such as different extension lengths or different URI types. For example, SIP URI dial plans require a SIP Resource Identifier to be assigned to each UM-enabled mailbox, but Telephone Extension dial plans don't. Also, each UM mailbox contains references to both the UM dial plan and the UM mailbox policy. The UM mailbox policy, in turn, contains references to the UM dial plan. If you change the primary proxy address for a UM-enabled user to point to a different dial plan, the UM mailbox is in an inconsistent state.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- Before you perform these procedures, confirm that the existing Exchange recipient is enabled for Unified Messaging. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Create the new UM dial plan

> [!IMPORTANT]
> If you're migrating UM-enabled users to Microsoft Office Communications Server 2007 R2 or to Microsoft Lync Server, you must first create a SIP URI dial plan.

For detailed instructions, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

## Step 2: Disable the user for Unified Messaging

For detailed instructions, see [Disable voice mail for a user](disable-voice-mail.md).

## Step 3: Enable the user for Unified Messaging on the new UM dial plan

> [!IMPORTANT]
> If you're moving users to an environment with Office Communications Server 2007 R2 or Lync Server, you must also include a SIP Resource Identifier for the user when you enable them for UM. You must also select the UM mailbox policy that's associated with a SIP dial plan.

For detailed instructions, see [Enable a user for voice mail](enable-a-user-for-voice-mail.md).
