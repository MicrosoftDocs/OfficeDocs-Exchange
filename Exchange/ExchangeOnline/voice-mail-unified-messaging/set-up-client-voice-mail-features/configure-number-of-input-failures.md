---
localization_priority: Normal
description: You can configure the number of times that users who call in to an Outlook Voice Access number can enter incorrect data before they're disconnected. This setting applies to both Outlook Voice Access users and unauthenticated callers who use directory search.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 64c13d17-a26a-4c9b-b495-bd69c716456a
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure the number of input failures before Outlook Voice Access users are disconnected in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the number of input failures before Outlook Voice Access users are disconnected in Exchange Online

You can configure the number of times that users who call in to an Outlook Voice Access number can enter incorrect data before they're disconnected. This setting applies to both Outlook Voice Access users and unauthenticated callers who use directory search.

The following are examples of types of data that are considered incorrect:

- A caller requests an extension number that isn't found in the system.

- The system can't locate the user's extension number to transfer the call.

- A caller presses a menu option that isn't valid.

The value of this setting can be from 1 through 20. For most organizations, this value should be set to the default of three attempts. Setting this value too low may prematurely disconnect callers.

For additional management tasks related to UM dial plans, see [UM dial plan procedures in Exchange Online](../connect-voice-mail-system/um-dial-plan-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure the input failures before disconnect

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Settings**, under **Number of input failures before disconnecting**, enter the number of input failures.

5. Click **Save**.

## Use Exchange Online PowerShell to configure the input failures before disconnect

This example sets the input failures before disconnect to 5 on a UM dial plan named `MyUMDialPlan`.

```PowerShell
Set-UMDialPlan -identity MyUMDialPlan -InputFailuresBeforeDisconnect 5
```
