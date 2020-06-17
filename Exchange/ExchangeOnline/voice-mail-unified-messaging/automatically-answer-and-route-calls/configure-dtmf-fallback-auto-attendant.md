---
localization_priority: Normal
description: You can configure a speech-enabled Unified Messaging (UM) auto attendant that has a dual tone multi-frequency (DTMF) fallback auto attendant. A DTMF fallback auto attendant is used when the UM speech-enabled auto attendant can't understand or recognize the speech inputs provided by a caller. If a DTMF fallback auto attendant has been configured, the caller has to use DTMF inputs, also known as touchtone inputs, to navigate the auto attendant menu system, spell a user's name, or use a custom menu prompt. If no DTMF fallback auto attendant has been configured, and the maximum number of speech inputs is exceeded because the system didn't understand what the caller said, the system will respond with this prompt:Sorry, I couldn't help. Please call back later.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a82d85f7-de30-40db-8ee6-b091ac14da9d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure a DTMF fallback auto attendant in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure a DTMF fallback auto attendant in Exchange Online

You can configure a speech-enabled Unified Messaging (UM) auto attendant that has a dual tone multi-frequency (DTMF) fallback auto attendant. A DTMF fallback auto attendant is used when the UM speech-enabled auto attendant can't understand or recognize the speech inputs provided by a caller. If a DTMF fallback auto attendant has been configured, the caller has to use DTMF inputs, also known as touchtone inputs, to navigate the auto attendant menu system, spell a user's name, or use a custom menu prompt. If no DTMF fallback auto attendant has been configured, and the maximum number of speech inputs is exceeded because the system didn't understand what the caller said, the system will respond with this prompt: "Sorry, I couldn't help. Please call back later."

By default, an auto attendant isn't speech-enabled when you create it. After you speech-enable the auto attendant, callers can use only voice commands to navigate the auto attendant menu system, and touchtone inputs can't be used. Although it isn't required, we recommend that you configure a DTMF fallback auto attendant for each speech-enabled auto attendant so callers can use touchtone inputs if the speech-enabled auto attendant doesn't recognize or understand the words they say. We also recommend that you don't speech-enable a DTMF fallback auto attendant.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure a speech-enabled auto attendant with a DTMF fallback auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to create a DTMF fallback auto attendant. On the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **General**, select the check box next to **Use this auto attendant when voice commands don't work correctly**, and then click **Browse**.

4. On the **Select a UM Auto Attendant** page, select the auto attendant you want to use as a DTMF fallback auto attendant, and then click **Save**.

> [!IMPORTANT]
> You must first speech-enable the auto attendant before you can browse for a DTMF fallback auto attendant you have set up.

## Use Exchange Online PowerShell to configure a speech-enabled auto attendant with a DTMF fallback auto attendant

This example configures a UM auto attendant named `MySpeechEnabledAA` to use a DTMF fallback auto attendant named `MyDTMFAA`.

```PowerShell
Set-UMAutoAttendant -Identity MySpeechEnabledAA -DTMFFallbackAutoAttendant MyDTMFAA
```
