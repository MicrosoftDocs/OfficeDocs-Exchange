---
localization_priority: Normal
description: By default, each Unified Messaging (UM) dial plan uses a standard .wav file for the welcome greeting that's played to callers, including Outlook Voice Access users who dial in to an Outlook Voice Access number that's been configured. However, you can create a .wav or .wma file for the welcome greeting, and then enable it on the UM dial plan.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: abd418ec-2c65-4720-859d-c11a2698dc06
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable a customized greeting for Outlook Voice Access users in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable a customized greeting for Outlook Voice Access users in Exchange Online

By default, each Unified Messaging (UM) dial plan uses a standard .wav file for the welcome greeting that's played to callers, including Outlook Voice Access users who dial in to an Outlook Voice Access number that's been configured. However, you can create a .wav or .wma file for the welcome greeting, and then enable it on the UM dial plan.

For example, you might want to change the default welcome greeting and instead provide a welcome greeting that's specific to your company, such as "Welcome to Outlook Voice Access for Woodgrove Bank." To do this, you record the customized welcome greeting and save it as a .wav or .wma file. Then you configure the dial plan to use the customized welcome greeting.

For more information about the menu options available for Outlook Voice Access users, see the Quick Reference Guide for Outlook Voice Access, which is available from the [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?linkId=272767).

For additional management tasks related to UM dial plans, see [Dial Plan Procedures](https://technet.microsoft.com/library/1bda77c8-c4e2-4ae0-a001-76ae029bf843.aspx).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable a customized welcome greeting

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan that you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Outlook Voice Access**, under **Welcome greeting**, click **Change**, and then click **Browse** to locate the greeting file.

    > [!IMPORTANT]
    > The file you use for the welcome greeting must be a .wav or .wma file.

5. After you've located the file, click **Open**, and then click **Save**.

## Use Exchange Online PowerShell to enable a customized welcome greeting

This example enables a welcome greeting that uses the C:\UMPrompts\welcome.wav file on a UM dial plan named `MyUMDialPlan`.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -WelcomeGreetingEnabled $true -WelcomeGreetingFilename c:\UMPrompts\welcome.wav
```
