---
localization_priority: Normal
description: You can configure the method to use for users with similar names on an auto attendant's Address book and operator access options, or you can leave the default setting on the auto attendant and configure this setting on the dial plan associated with the auto attendant. By default, an auto attendant can disambiguate between two or more users who have the same or similar names because the default setting on the auto attendant is Inherit from dial plan.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 2e7318a0-67f9-4d7b-8300-5f0ef77656a8
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure an auto attendant for users who have similar names in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure an auto attendant for users who have similar names in Exchange Online

You can configure the method to use for users with similar names on an auto attendant's **Address book and operator access** options, or you can leave the default setting on the auto attendant and configure this setting on the dial plan associated with the auto attendant. By default, an auto attendant can disambiguate between two or more users who have the same or similar names because the default setting on the auto attendant is **Inherit from dial plan**.

> [!NOTE]
> For the information that will be included for users with similar names to work correctly, you must provide the title, department, and location information for the recipients in your Microsoft Exchange organization.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure a UM auto attendant for users with similar names

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to configure, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page, click **Address book and operator access**, and under **Information to include for users with the same name**, select one of the following:

   - **Title**: The auto attendant will include each user's title when it lists matches.

   - **Department**: The auto attendant will include each user's department when it lists matches.

   - **Location**: The auto attendant will include each user's location when it lists matches.

   - **None**: The auto attendant won't include any additional information when it lists matches.

   - **Prompt For alias**: The auto attendant will prompt the caller for the user's alias.

   - **Inherit from dial plan**: The auto attendant will use the default setting from the dial plan associated with the auto attendant.

4. Click **Save**.

## Use Exchange Online PowerShell to configure a UM auto attendant for users with similar names

This example sets the information to be included with users with similar names to Prompt for Alias for a UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -MatchedNameSelectionMethod PromptForAlias
```

This example sets the information to be included with users with similar names to the title of the users, enables name lookups, and enables callers that dial into the auto attendant to press \* to be presented with the Outlook Voice Access welcome greeting for a UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -MatchedNameSelectionMethod Title -NameLookupEnabled $true -StarOutToDialPlanEnabled $true
```
