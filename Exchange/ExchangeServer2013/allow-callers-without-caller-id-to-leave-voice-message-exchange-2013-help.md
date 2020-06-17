---
title: 'Allow callers without a caller ID to leave a voice message: Exchange 2013 Help'
TOCTitle: Allow callers without a caller ID to leave a voice message
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 51367d98-e17c-4bcf-8b14-208bd1ac3af0
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Allow callers without a caller ID to leave a voice message in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can allow UM-enabled users to receive voice mail messages from anonymous callers or prevent them from doing so. By default, when users are enabled for Unified Messaging (UM) and voice mail, they can receive calls that are anonymous and don't contain caller ID information.

In most cases, calls received by Unified Messaging contain a caller ID that can be used to determine the source of the incoming call. However, incoming calls may not include caller ID information for the following reasons:

- Your organization's telephony equipment is configured not to include caller ID information.

- The incoming call is from a mobile or external telephone.

- The caller has disabled caller ID on their telephone.

Because the _AnonymousCallersCanLeaveMessages_ parameter is enabled by default, a UM-enabled user can receive a voice message even if caller ID information isn't included. If the _AnonymousCallersCanLeaveMessages_ option is disabled, and the UM-enabled user receives a call that doesn't include a caller ID, the call will be identified as anonymous, and the UM-enabled user won't receive a voice message.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform this procedure, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to allow voice messages from anonymous callers to be received

This example allows UM-enabled user tonysmith@contoso.com to receive voice messages from incoming calls that don't contain caller ID information.

```powershell
Set-UMMailbox -Identity tonysmith@contoso.com -AnonymousCallersCanLeaveMessages $true
```
