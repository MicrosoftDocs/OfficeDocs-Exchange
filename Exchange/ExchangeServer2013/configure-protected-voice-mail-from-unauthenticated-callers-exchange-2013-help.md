---
title: 'Configure Protected Voice Mail from unauthenticated callers: Exchange 2013 Help'
TOCTitle: Configure Protected Voice Mail from unauthenticated callers
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 106bfa0a-a0fa-4a1b-bd59-4b6df1d0d61d
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Protected Voice Mail from unauthenticated callers in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can configure Unified Messaging to answer an incoming call, and then determine whether it will apply protection to voice mail messages by using encryption. When a voice mail message is protected:

- The message is marked as Private in Microsoft Outlook and Outlook Web App.

- The voice message can be opened only by the intended recipient of the voice message.

- The recipient can reply to the voice message, but can't forward it to someone who wasn't included on the original voice message.

This setting applies to voice messages sent to UM-enabled users when they don't answer their phone. This setting also applies to voice messages sent directly to UM-enabled users when the caller uses a UM auto attendant.

For additional management tasks related to Protected Voice Mail procedures, see [Protected Voice Mail procedures](protected-voice-mail-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure Protected Voice Mail from unauthenticated callers

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Protected voice mail**, under **Protect voice message from unauthenticated callers**, select one of the following options:

   - **None**: Use this setting when you don't want protection applied to any voice messages sent to UM-enabled users.

   - **Private**: Use this setting when you want Unified Messaging to apply protection only to voice messages that have been marked as private by the caller.

   - **All**: Use this setting when you want Unified Messaging to apply protection to all voice messages, including those not marked as private.

4. Click **Save**.

## Use the Shell to configure Protected Voice Mail from unauthenticated callers

This example protects all voice messages from all unauthenticated callers on the UM mailbox policy `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -ProtectUnauthenticatedVoiceMail -All
```
