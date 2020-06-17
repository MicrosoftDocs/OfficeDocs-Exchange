---
localization_priority: Normal
description: You can use the New menu navigation entry page to create single or multiple key mappings for business or non-business hours main menu prompts for auto attendants. You can define the action that will be performed when a key on the telephone keypad is pressed, for example, transferring the call to an extension number or another auto attendant.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3cfc9a01-0a61-4d15-9561-621568dc30d9
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.UnifiedMessaging.AutoAttendantKeyMappingControl
title: Create menu navigation in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create menu navigation in Exchange Online

You can use the **New menu navigation entry** page to create single or multiple key mappings for business or non-business hours main menu prompts for auto attendants. You can define the action that will be performed when a key on the telephone keypad is pressed, for example, transferring the call to an extension number or another auto attendant.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure UM auto attendant navigation menus

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to create menu navigation. On the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page, click **Menu navigation**, select either **Enable business hours menu navigation** or **Enable non-business hours menu navigation**, and then click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

4. On the **New menu navigation entry** page, configure the following:

   - **Prompt**: Use this box to type the name of the new navigation menu. The navigation menu name is used for display purposes only. This is a required field.

     Because you may want to specify multiple new navigation menus, we recommend that you use meaningful names for your key mappings. The maximum length of the name for the key mapping is 64 characters, and it can include spaces. However, it can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

   - **When this key is pressed**: Use this list to enable key mapping. The key mapping is the number key that a caller presses to have the auto attendant perform a specific operation, for example, forwarding the caller to another auto attendant or to an operator. By default, no entries are defined.

     Use the drop-down list to select the numeric key (from 1 through 9) that the caller must press. Zero (0) is reserved for the auto attendant operator.

     If you select **Time Out** from the drop down list, it enables callers to be transferred to an extension number or to another auto attendant if they don't press a key on the telephone keypad. For example, "Please stay on the line and your call will be answered by the next available representative." The default setting is 5 seconds. If you enable this option, a blank key mapping will be created.

   - **Play the following audio file**: Use this option to select a previously recorded audio file for callers. Click **Change**, and then click **Browse** to locate the audio file.

   - **Perform this additional action**: Select one of the following options to define the action that you want the auto attendant to perform for the caller:

   - **None**: If you don't want to the auto attendant to transfer the call to an extension or to another auto attendant, or leave a message for a user, use this option.

   - **Transfer to this extension**: Select this option to enable calls to be transferred to an extension number. If you enable this option, use the box to type the extension where the call will be transferred. This field allows only numeric characters. It can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

   - **Transfer to this UM auto attendant**: Select this option to transfer the call to an auto attendant. Click **Browse** to locate the auto attendant that you want to use. Before you enable this option, you must first create and configure the auto attendant. This option is used when you create a parent/child structure of UM auto attendants.

   - **Leave a voice message for this user**: Select this option to enable a caller to leave a voice mail message for a user that's on the same dial plan as the UM auto attendant that you're configuring. When a caller chooses this option from an auto attendant menu, they'll be prompted to leave a voice message for the user that was selected. Click **Browse** to locate the UM-enabled user.

   - **Announce business location**: Select this option to enable a caller to choose an auto attendant menu option and hear the location of the business that's configured on the UM auto attendant. To enable this to work correctly, you must first enter the business location in the **Business location** box on the **General** page on the UM auto attendant.

   - **Announce business hours**: Select this option to enable a caller to choose an auto attendant menu option and hear the hours of operation for the business that's configured on the UM auto attendant. To enable this to work correctly, you must first configure the business hours on the **Business hours** page on the UM auto attendant.

5. Click **OK** to create the new menu navigation.

6. On the **UM Auto Attendant** page, click **Save** to save your changes.

## Use Exchange Online PowerShell to configure UM auto attendant key mappings

This example enables business hours key mappings so that:

- When callers press 1, they will be forwarded to another UM auto attendant named `SalesAutoAttendant`.

- When they press 2, they will be forwarded to extension number 12345 for Support.

- When they press 3, they will be sent to another auto attendant that will play an audio file.

```PowerShell
Set-UMAutoAttendant -Identity MyAutoAttendant -BusinessHoursKeyMappingEnabled $true -BusinessHoursKeyMapping "1,Sales,,SalesAutoAttendant","2,Support,12345","3,Directions,,,directions.wav"
```

This example sets key mappings defined in a comma-separated value (.csv) file. You must first create the .csv file with the following headings and the correct entry: \<key\>,\<description\>,[\<extension\>],[\<autoattendant name\>],[\<promptfilenamepath\>],[\<asrphrase1;asrphrase2\>],[\<leavevoicemailfor\>],[\<transfertomailbox\>]. The values in brackets are optional. After creating the .csv file, import the .csv file using the **Import-csv** cmdlet.

```PowerShell
$o = Import-csv -path "C:\UMFiles\AutoAttendants\keymappings.csv"
Set-UMAutoAttendant MyAutoAttendant -BusinessHoursKeyMapping $o
```

This example exports key mappings from an existing UM auto attendant into a .csv file, and then imports the same key mappings into another UM auto attendant. You could also export the key mappings to a .csv file, edit or modify the key mappings in the .csv file, and then import those key mappings into another UM auto attendant.

```PowerShell
$aa = Get-UMAutoAttendant -Identity MyAutoAttendant
$aa1 = Get-UMAutoAttendant -Identity MyAutoAttendant2
$aa.BusinessHoursKeyMapping | Export-csv -path "C:\UMFiles\AutoAttendants\keymappings.csv"
$aa1.BusinessHoursKeyMapping = (Import-csv -path "C:\UMFiles\AutoAttendants\keymappings.csv")
```
