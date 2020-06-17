---
localization_priority: Normal
description: When you configure business hours for a Unified Messaging (UM) auto attendant, you define the hours of the day that your organization is open, and the business hours greetings and menu prompts callers will hear when they call an extension number that's configured on the auto attendant. If a caller reaches the auto attendant during hours that are outside the business hours you define, the caller will hear the non-business hours prompts and greetings.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 96b4be99-af94-4fa4-959a-48413387a044
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure business hours in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure business hours in Exchange Online

When you configure business hours for a Unified Messaging (UM) auto attendant, you define the hours of the day that your organization is open, and the business hours greetings and menu prompts callers will hear when they call an extension number that's configured on the auto attendant. If a caller reaches the auto attendant during hours that are outside the business hours you define, the caller will hear the non-business hours prompts and greetings.

Several default schedule options are available in the EAC. For example, most businesses are open from 8:00 A.M. to 5:00 P.M., Monday through Friday. Sometimes the default options won't fit your needs and you'll want to customize the schedule. If your business hours vary from the schedules defined by the system, you can define a customized schedule for the auto attendant.

By default, the UM auto attendant will play the business hours prompts and greetings regardless of the time of day callers dial in to the auto attendant.

> [!NOTE]
> When you set the schedule for business and non-business hours on a UM auto attendant, make sure the time zone is configured correctly.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to specify business hours for a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to set the business hours, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Business Hours** under **Business hours**, click **Configure business hours**.

4. On the **Configure Business Hours** page, select the hours you want to use as your business hours for each day of the week.

5. Click **OK**, and then click **Save**.

## Use Exchange Online PowerShell to specify business hours for a UM auto attendant

This example sets the business hours for a UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessHoursSchedule 0.10:45-0.13:15,1.09:00-1.17:00,6.09:00-6.16:30
```
