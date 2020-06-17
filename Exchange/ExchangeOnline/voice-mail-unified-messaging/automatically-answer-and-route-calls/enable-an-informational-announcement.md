---
localization_priority: Normal
description: You can enable an informational announcement for a Unified Messaging (UM) auto attendant. When an informational announcement is enabled, it will play immediately after the business or non-business hours greeting. By default, an informational announcement isn't configured. To enable an informational announcement, create a .wav or .wma file to be used as the informational announcement, and then configure the auto attendant to use this sound file.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 07f6c13e-3781-4127-9321-f0f85f054259
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable an informational announcement in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable an informational announcement in Exchange Online

You can enable an informational announcement for a Unified Messaging (UM) auto attendant. When an informational announcement is enabled, it will play immediately after the business or non-business hours greeting. By default, an informational announcement isn't configured. To enable an informational announcement, create a .wav or .wma file to be used as the informational announcement, and then configure the auto attendant to use this sound file.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- Create a .wav or .wma file to be used for the informational announcement.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable an informational announcement

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan that you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to enable an informational announcement, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page, \> **Greetings**, under **Informational announcement** click **Change**, and then click **Browse** to locate the informational announcement file you created before you started this procedure.

    > [!IMPORTANT]
    > The file you use for the greeting must be a .wav or .wma file.

4. After you've located the file, click **Open**, and then click **Save**.

## Use Exchange Online PowerShell to enable an informational announcement

This example enables an informational announcement that uses the `MyInfoAnnouncement.wav` file for the UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -InfoAnnouncementEnabled $true -InfoAnnouncementFilename MyInfoAnnouncement.wav
```
