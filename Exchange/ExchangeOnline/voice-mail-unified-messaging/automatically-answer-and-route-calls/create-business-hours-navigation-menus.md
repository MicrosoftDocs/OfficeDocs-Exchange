---
localization_priority: Normal
description: You can enable business hours key mappings for a Unified Messaging (UM) auto attendant. After you create a UM auto attendant, a default system prompt will be used for the business hours main menu prompt greeting that callers hear after the business hours welcome greeting is played. The default business hours main menu prompt says,Welcome to the Microsoft Exchange auto attendant.Because no key mappings are defined by default, no menu options are available to callers, and they hear only the default main menu prompt.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f76472fd-aa1a-4cd8-8e26-cc674421d375
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create business hours navigation menus in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create business hours navigation menus in Exchange Online

You can enable business hours key mappings for a Unified Messaging (UM) auto attendant. After you create a UM auto attendant, a default system prompt will be used for the business hours main menu prompt greeting that callers hear after the business hours welcome greeting is played. The default business hours main menu prompt says, "Welcome to the Microsoft Exchange auto attendant." Because no key mappings are defined by default, no menu options are available to callers, and they hear only the default main menu prompt.

When you configure key mappings, you define the options and the operations that will be performed if a caller speaks a phrase while they're using a speech-enabled auto attendant or presses a key on the telephone keypad while they're using an auto attendant that isn't speech-enabled.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable business hours key mappings on a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to create a business hours navigation menu. On the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page, click **Menu navigation**, under **Business hours menu navigation**, select **Enable business hours menu navigation**, and then click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

4. On the **New menu navigation entry** page, use the following options to create a new navigation entry:

   - **Prompt**: Use this box to type the name of the new navigation menu. The navigation menu name is used for display purposes only. This is a required field.

     Because you may want to specify multiple new navigation menus, we recommend that you use meaningful names for your key mappings. The maximum length of the name for the key mapping is 64 characters, and it can include spaces. However, it can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

   - **When this key is pressed**: Use this list to enable key mapping. The key mapping is the number key that a caller presses to have the auto attendant perform a specific operation, for example, forwarding the caller to another auto attendant or to an operator. By default, no entries are defined.

     Use the drop down list to select the numeric key (from 1 through 9) that the caller must press. Zero (0) is reserved for the auto attendant operator.

     If you select **Time Out** from the drop down list, it enables callers to be transferred to an extension number or to another auto attendant if they don't press a key on the telephone keypad. For example, "Please stay on the line and your call will be answered by the next available representative." The default setting is 5 seconds. If you enable this option, a blank key mapping will be created.

   - **Play the following audio file**: Use this option to select a previously recorded audio file for callers. Click **Change**, and then click **Browse** to locate the audio file. If you leave the audio file as the default \<None\>, the Unified Messaging TTS (Text to Speech) engine will synthesize a business hours main menu prompt. Alternatively, you can create a customized audio file that can be used for the business hours main menu prompt for a speech-enabled auto attendant. For example, it might say, "To leave a voice message for sales, say 1. To leave a voice message for technical support, say 2. To leave a voice message for administration, say 3."

   - **Perform this additional action**: Select one of the following options to define the action that you want the auto attendant to perform for the caller:

   - **None**: If you don't want the auto attendant to transfer the call to an extension or to another auto attendant, or leave a message for a user, use this option.

   - **Transfer to this extension**: Select this option to enable calls to be transferred to an extension number. If you enable this option, use the box to type the extension number where the call will be transferred. This field allows only numeric characters. It can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

   - **Transfer to this UM auto attendant**: Select this option to transfer the call to an auto attendant. Click **Browse** to locate the auto attendant that you want to use. Before you enable this option, you must first create and configure the auto attendant. This option is used when you create a parent/child structure of UM auto attendants.

   - **Leave a voice message for this user**: Select this option to enable a caller to leave a voice mail message for a user that's on the same dial plan as the UM auto attendant that you're configuring. When a caller chooses this option from an auto attendant menu, they'll be prompted to leave a voice message for the user that was selected. Click **Browse** to locate the UM-enabled user.

   - **Announce business location**: Select this option to enable a caller to choose an auto attendant menu option and hear the location of the business that's configured on the UM auto attendant. To enable this to work correctly, you must first enter the business location in the **Business location** box on the **General** page on the UM auto attendant.

   - **Announce business hours**: Select this option to enable a caller to choose an auto attendant menu option and hear the hours of operation for the business that's configured on the UM auto attendant. To enable this to work correctly, you must first configure the business hours on the **Business hours** page on the UM auto attendant.

5. Click **OK** to create the new menu navigation.

6. On the **UM Auto Attendant** page, click **Save** to save your changes.

## Use Exchange Online PowerShell to enable business hours key mappings on a UM auto attendant

This example configures a UM auto attendant named `MyAutoAttendant` and enables business hours key mappings so that when callers press 1, they're forwarded to another UM auto attendant named `SalesAutoAttendant`. When they press 2, they're forwarded to extension number 12345 for Support, and when they press 3, they're sent to another auto attendant that plays an audio file.

```PowerShell
Set-UMAutoAttendant -Identity MyAutoAttendant - BusinessHoursKeyMappingEnabled $true -BusinessHoursKeyMapping "1,Sales,,SalesAutoAttendant","2,Support,12345","3,Directions,,,directions.wav"
```
