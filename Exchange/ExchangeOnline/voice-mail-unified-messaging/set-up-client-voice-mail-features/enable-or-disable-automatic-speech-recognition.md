---
localization_priority: Normal
description: You can configure Automatic Speech Recognition (ASR) for a user who's enabled for Unified Messaging (UM) and voice mail. When ASR is enabled on the mailbox of an Outlook Voice Access user, the user can move through the mailbox menus using voice commands. ASR is enabled by default. If ASR is disabled, the user must use dual tone multi-frequency (DTMF), also known as touchtone, inputs to move through the menus.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 58f41016-e725-432b-953e-415d61e0664c
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable automatic speech recognition for an Outlook Voice Access user in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable automatic speech recognition for an Outlook Voice Access user in Exchange Online

You can configure Automatic Speech Recognition (ASR) for a user who's enabled for Unified Messaging (UM) and voice mail. When ASR is enabled on the mailbox of an Outlook Voice Access user, the user can move through the mailbox menus using voice commands. ASR is enabled by default. If ASR is disabled, the user must use dual tone multi-frequency (DTMF), also known as touchtone, inputs to move through the menus.

> [!NOTE]
> You can't use the EAC to configure this feature. You must use Exchange Online PowerShell to enable or disable ASR for a voice mail user.

For additional management tasks related to UM or voice mail users, see [Voice mail-enabled user procedures](../../voice-mail-unified-messaging/set-up-voice-mail/voice-mail-enabled-user-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- Before you perform these procedures, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](../../voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to enable or disable ASR for a UM-enabled user

This example enables ASR for a UM-enabled user named `tonysmith`.

```PowerShell
Set-UMMailbox -Identity tonysmith@contoso.com -AutomaticSpeechRecognitionEnabled $true
```

This example disables ASR for a UM-enabled user named `tonysmith`.

```PowerShell
Set-UMMailbox -Identity tonysmith@contoso.com -AutomaticSpeechRecognitionEnabled $false
```
