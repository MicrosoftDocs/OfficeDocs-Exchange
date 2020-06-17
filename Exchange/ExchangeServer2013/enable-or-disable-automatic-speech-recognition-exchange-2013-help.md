---
title: 'Enable or disable automatic speech recognition for an Outlook Voice Access user: Exchange 2013 Help'
TOCTitle: Enable or disable automatic speech recognition for an Outlook Voice Access user
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 58f41016-e725-432b-953e-415d61e0664c
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable automatic speech recognition for an Outlook Voice Access user in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can configure Automatic Speech Recognition (ASR) for a user who's enabled for Unified Messaging (UM) and voice mail. When ASR is enabled on the mailbox of an Outlook Voice Access user, the user can move through the mailbox menus using voice commands. ASR is enabled by default. If ASR is disabled, the user must use dual tone multi-frequency (DTMF), also known as touchtone, inputs to move through the menus.

> [!NOTE]
> You can't use the EAC to configure this feature. You must use the Shell to enable or disable ASR for a voice mail user.

For additional management tasks related to UM or voice mail users, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform these procedures, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to enable or disable ASR for a UM-enabled user

This example enables ASR for a UM-enabled user named `tonysmith`.

```powershell
Set-UMMailbox -Identity tonysmith@contoso.com -AutomaticSpeechRecognitionEnabled $true
```

This example disables ASR for a UM-enabled user named `tonysmith`.

```powershell
Set-UMMailbox -Identity tonysmith@contoso.com -AutomaticSpeechRecognitionEnabled $false
```
