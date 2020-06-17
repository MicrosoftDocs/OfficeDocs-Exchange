---
title: 'Enable a customized non-business hours menu prompt: Exchange 2013 Help'
TOCTitle: Enable a customized non-business hours menu prompt
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 094c50b2-072b-4929-aaf8-f7db5b19e9b6
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable a customized non-business hours menu prompt in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can customize the menu prompt to be used by a Unified Messaging (UM) auto attendant outside business hours. After you create a UM auto attendant, a default system prompt ("Welcome to Unified Messaging") is used as the menu prompt that callers hear after the non-business hours welcome greeting is played. Although the system prompt mustn't be replaced or changed, you can customize the greetings and menu prompts that are used with UM auto attendants. After you create a customized non-business hours menu prompt audio file, you must enable menu navigation entries on the UM auto attendant for non-business hours.

If you only want to include the name of your organization or business as part of the default system prompt, you can enter the name in the **Business name** box on the UM auto attendant. For details, see [Enter a business name](enter-a-business-name-exchange-2013-help.md).

> [!IMPORTANT]
> You must configure business hours on the auto attendant. When you configure business hours, the non-business hours are set automatically. For details, see [Configure business hours](configure-business-hours-exchange-2013-help.md).

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- Create a .wav or .wma file to be used for the menu prompt.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable a customized non-business hours menu prompt

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan that you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to enable a customized non-business hours menu prompt, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Menu navigation**, under **Non-business hours menu navigation**, click **Change**, and then click **Browse** to locate the customized non-business hours menu prompt file.

    > [!IMPORTANT]
    > The file you use for the menu prompt must be a .wav or .wma file.

4. After you've located the file, click **Open**, and then click **Save**.

## Use the Shell to enable a customized non-business hours menu prompt

This example enables a UM auto attendant named `MyUMAutoAttendant` that has business hours configured to be 10:45 to 13:15 (Sunday), 09:00 to 17:00 (Monday), and 09:00 to 16:30 (Saturday) and holiday times and their associated greetings configured to be `New Year` on January 1, 2013, and `Building Closed for Construction` from April 24, 2013 through April 28, 2013.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessHoursSchedule 0.10:45-0.13:15,1.09:00-1.17:00,6.09:00-6.16:30 -HolidaySchedule "New Year,newyrgrt.wav,1/2/2013","Building Closed for Construction,construction.wav,4/24/2013,4/28/2013"
```

This example configures a UM auto attendant named `MyAutoAttendant` and enables non-business hours navigation menus so that when callers press 1, they're forwarded to another UM auto attendant named `SalesAutoAttendant`. When they press 2, they're forwarded to extension number 12345 for `Support`, and when they press 3, they're sent to another UM auto attendant that plays an audio file.

```powershell
Set-UMAutoAttendant -Identity MyAutoAttendant -
AfterHoursKeyMappingEnabled $true -
AfterHoursKeyMapping "1,Sales,,SalesAutoAttendant","2,Support,12345","3,Directions,,,directions.wav"
```
