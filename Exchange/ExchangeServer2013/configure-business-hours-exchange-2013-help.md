---
title: 'Configure business hours: Exchange 2013 Help'
TOCTitle: Configure business hours
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 96b4be99-af94-4fa4-959a-48413387a044
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure business hours in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you configure business hours for a Unified Messaging (UM) auto attendant, you define the hours of the day that your organization is open, and the business hours greetings and menu prompts callers will hear when they call an extension number that's configured on the auto attendant. If a caller reaches the auto attendant during hours that are outside the business hours you define, the caller will hear the non-business hours prompts and greetings.

Several default schedule options are available in the EAC. For example, most businesses are open from 8:00 A.M. to 5:00 P.M., Monday through Friday. Sometimes the default options won't fit your needs and you'll want to customize the schedule. If your business hours vary from the schedules defined by the system, you can define a customized schedule for the auto attendant.

By default, the UM auto attendant will play the business hours prompts and greetings regardless of the time of day callers dial in to the auto attendant.

> [!NOTE]
> When you set the schedule for business and non-business hours on a UM auto attendant, make sure the time zone is configured correctly.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to specify business hours for a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to set the business hours, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Business Hours** under **Business hours**, click **Configure business hours**.

4. On the **Configure Business Hours** page, select the hours you want to use as your business hours for each day of the week.

5. Click **OK**, and then click **Save**.

## Use the Shell to specify business hours for a UM auto attendant

This example sets the business hours for a UM auto attendant named `MyUMAutoAttendant`.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessHoursSchedule 0.10:45-0.13:15,1.09:00-1.17:00,6.09:00-6.16:30
```
