---
localization_priority: Normal
description: You can enable your Unified Messaging (UM) auto attendant for Automatic Speech Recognition (ASR). After you speech-enable a UM auto attendant, callers can respond verbally to auto attendant prompts and move through the menu system of the auto attendant. By default, an auto attendant isn't speech-enabled when you create it. After you speech-enable the auto attendant, callers can use only voice commands to navigate the auto attendant menu system, and touchtone inputs can't be used.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 92b3b679-b503-4068-8e88-25ec0f4537ab
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable automatic speech recognition in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable automatic speech recognition in Exchange Online

You can enable your Unified Messaging (UM) auto attendant for Automatic Speech Recognition (ASR). After you speech-enable a UM auto attendant, callers can respond verbally to auto attendant prompts and move through the menu system of the auto attendant. By default, an auto attendant isn't speech-enabled when you create it. After you speech-enable the auto attendant, callers can use only voice commands to navigate the auto attendant menu system, and touchtone inputs can't be used.

Although it isn't required, we recommend that you configure a dual tone multi-frequency (DTMF) fallback auto attendant for each speech-enabled auto attendant so callers can use touchtone inputs if the speech-enabled auto attendant doesn't recognize or understand the words they say. If a DTMF fallback auto attendant is configured, callers can use DTMF inputs, also known as touchtone inputs, to navigate the auto attendant menu system, spell a user's name, or use a custom menu prompt. We don't recommend that you speech-enable a DTMF fallback auto attendant.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to speech-enable a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to speech enable, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **General**, select the check box next to **Set the auto attendant to respond to voice commands** to enable speech recognition. To disable automatic speech recognition, clear this check box.

4. Click **Save**.

## Use Exchange Online PowerShell to speech-enable a UM auto attendant

This example enables ASR on a UM auto attendant named `MySpeechEnabled AA`.

```PowerShell
Set-UMAutoAttendant -Identity MySpeechEnabledAA -SpeechEnabled $true
```
