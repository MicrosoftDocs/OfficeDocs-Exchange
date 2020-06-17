---
localization_priority: Normal
description: You can specify the maximum number of minutes allowed for each voice recording when a caller leaves a voice mail message. This value can be set to a number from 1 through 100. For most organizations, this value should be set to the default of 20 minutes. Setting this value too low can cause long voice messages to be disconnected before they're completed. Setting this value too high lets users save lengthy voice messages in their Inboxes.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 18eeb567-1048-4c82-93cf-612cb12ec5e3
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure the maximum recording duration in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the maximum recording duration in Exchange Online

You can specify the maximum number of minutes allowed for each voice recording when a caller leaves a voice mail message. This value can be set to a number from 1 through 100. For most organizations, this value should be set to the default of 20 minutes. Setting this value too low can cause long voice messages to be disconnected before they're completed. Setting this value too high lets users save lengthy voice messages in their Inboxes.

This setting is important if you've implemented strict disk quotas for users. It must be set to a lower value than the one set for **Maximum call duration (minutes)**.

For additional tasks related to UM dial plans, see [UM dial plan procedures in Exchange Online](um-dial-plan-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure the maximum recording duration

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Settings**, under **Maximum recording duration (minutes)**, enter the number in minutes.

5. Click **Save**.

## Use Exchange Online PowerShell to configure the maximum recording duration

This example sets the maximum recording duration to 10 minutes for a UM dial plan named `MyUMDialPlan`.

```PowerShell
Set-UMDialPlan -identity MyUMDialPlan -MaxRecordingDuration 10
```
