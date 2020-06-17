---
localization_priority: Normal
description: 'You can specify the maximum number of minutes that an incoming call can be connected to the system without being transferred to a valid extension number before the call is ended. For most organizations, this value should be set to the default: 30 minutes. This setting applies to all calls, including incoming Outlook Voice Access calls, voice calls internal to your organization, voice calls into Unified Messaging (UM) auto attendants, and fax calls placed from outside your organization.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 01aa40d2-f918-472b-bace-158222143484
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure the maximum call duration in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the maximum call duration in Exchange Online

You can specify the maximum number of minutes that an incoming call can be connected to the system without being transferred to a valid extension number before the call is ended. For most organizations, this value should be set to the default: 30 minutes. This setting applies to all calls, including incoming Outlook Voice Access calls, voice calls internal to your organization, voice calls into Unified Messaging (UM) auto attendants, and fax calls placed from outside your organization.

This value can be set to a number from 10 through 120. Setting this value too low can cause incoming calls to be disconnected before they're completed. For example, if your organization receives many large fax messages, you may want to consider increasing this value from the default so that all the pages of fax messages are received.

For additional tasks related to UM dial plans, see [UM dial plan procedures in Exchange Online](um-dial-plan-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure the maximum call duration

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Settings**, under **Maximum call duration (minutes)**, enter the number in minutes.

5. Click **Save**.

## Use Exchange Online PowerShell to configure the maximum call duration

This example sets the maximum call duration to 10 minutes on a UM dial plan named `MyUMDialPlan`.

```PowerShell
Set-UMDialPlan -identity MyUMDialPlan -MaxCallDuration 10
```
