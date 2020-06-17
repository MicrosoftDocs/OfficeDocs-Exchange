---
localization_priority: Normal
description: When you create a dial plan, you can configure the primary and secondary dial by name methods or ways that callers can search for names. Callers use these dial by name methods to look up names to locate and contact a user when they call in to an Outlook Voice Access number or when they call in to a UM auto attendant that's associated with the dial plan. Callers can use touchtone inputs to locate a UM-enabled user.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5cd4e0a0-d023-45a1-aa3c-b8dea6ec6d72
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure the secondary way for Outlook Voice Access users to search in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the secondary way for Outlook Voice Access users to search in Exchange Online

When you create a dial plan, you can configure the primary and secondary dial by name methods or ways that callers can search for names. Callers use these dial by name methods to look up names to locate and contact a user when they call in to an Outlook Voice Access number or when they call in to a UM auto attendant that's associated with the dial plan. Callers can use touchtone inputs to locate a UM-enabled user.

> [!NOTE]
> If **None** is selected as the secondary way for callers to search for names, only the primary way of searching for names will be available to callers who want to locate users. If you configure both the primary and secondary ways that callers can search for names, callers will be prompted for both ways.

For additional management tasks related to UM dial plans, see  [UM dial plan procedures in Exchange Online](../connect-voice-mail-system/um-dial-plan-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to change the secondary dial by name method

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Dial Plan** page, click **Configure**.

4. In **Settings**, under **Secondary way to search for names**, use the drop-down list to select the option you want:

   - **Last first** (default)

   - **First last**

   - **SMTP address**

   - **None**

5. Click **Save**.

## Use Exchange Online PowerShell to change the secondary dial by name method

This example sets the secondary dial by name method to `FirstLast`. This enables callers who call the Outlook Voice Access number or a UM auto attendant associated with the dial plan to search for a UM-enabled user by their first and then last name.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -DialByNameSecondary FirstLast
```

This example sets the secondary dial by name method to `LastFirst`. This enables callers who call the Outlook Voice Access number or a UM auto attendant associated with the dial plan to search for a UM-enabled user by their last and then first name.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -DialByNameSecondary LastFirst
```

This example sets the secondary dial by name method to `SMTP address`. This enables callers who call the Outlook Voice Access number or a UM auto attendant associated with the dial plan to search for a UM-enabled user by their SMTP address.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -DialByNameSecondary SMTPAddress
```

This example sets the secondary dial by name method to `None` and the primary dial by name method to `SMTP address`. This enables callers who call the Outlook Voice Access number or a UM auto attendant associated with the dial plan to search for a UM-enabled user by their SMTP address only.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -DialByNamePrimary SMTPAddress -DialByNameSecondary None
```
