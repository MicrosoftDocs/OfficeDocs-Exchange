---
localization_priority: Normal
description: You can specify the number of seconds of silence that the system allows when a voice message is being recorded before the call is ended. For most organizations, this value should be set to the default of 5 seconds.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a7fb9a09-fde9-447d-ad2c-95598405e99b
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure the recording idle time-out value in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the recording idle time-out value in Exchange Online

You can specify the number of seconds of silence that the system allows when a voice message is being recorded before the call is ended. For most organizations, this value should be set to the default of 5 seconds.

This value can be set from 2 through 10. Setting this value too low can cause the system to disconnect callers before they've finished leaving their voice messages. Setting this value too high allows lengthy silences in voice messages.

For additional management tasks related to UM dial plans, see [UM dial plan procedures in Exchange Online](um-dial-plan-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure the recording idle time-out value

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Settings**, under **Recording idle time out (seconds)**, enter the number in seconds.

5. Click **Save**.

## Use Exchange Online PowerShell to configure the recording idle time-out value

This example sets the recording idle time-out value to 10 for a UM dial plan named `MyUMDialPlan`.

```PowerShell
Set-UMDialPlan -identity MyUMDialPlan -RecordingIdleTimeout 10
```
