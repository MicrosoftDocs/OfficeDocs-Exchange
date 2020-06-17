---
title: 'Enable a customized non-business hours greeting: Exchange 2013 Help'
TOCTitle: Enable a customized non-business hours greeting
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: d4743805-bab0-4735-a1e0-2cea4e088e8c
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable a customized non-business hours greeting in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable a customized non-business hours greeting for a Unified Messaging (UM) auto attendant. The non-business hours greeting is the first thing callers hear when a UM auto attendant answers their call during non-business hours. You'll probably want to customize the greeting.

Unified Messaging includes a default system prompt for use during non-business hours. Although the default system prompt mustn't be replaced or changed, you may want to provide an customized greeting. You can create a customized greeting in the .wav or .wma file format to be used when callers call in to a UM auto attendant during non-business hours. For example, "You've reached Woodgrove Bank after hours."

If you want to include the name of your organization or business as part of the default greeting, you can enter the name in the **Business name** box on the UM auto attendant. For details, see [Enter a business name](enter-a-business-name-exchange-2013-help.md).

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- Create a .wav or .wma file to be used for the greeting.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable a customized non-business hours greeting

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to enable a customized non-business hours greeting, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page, \> **Greetings**, under **Non-business hours greeting**, click **Change**, and then click **Browse** to locate the customized non-business hours greeting file you created before you started this procedure.

    > [!IMPORTANT]
    > The file you use for the greeting must be a .wav or .wma file.

4. After you've located the file, click **Open**, and then click **Save**.

## Use the Shell to enable a customized non-business hours greeting

This example enables the non-business hours greeting that uses a customized greeting named `GreetingFile.wav` for the UM auto attendant `MyUMAutoAttendant`.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -AfterHoursWelcomeGreetingEnabled $true -AfterHoursWelcomeGreetingFilename GreetingFile.wav
```

This example configures a UM auto attendant named `MyUMAutoAttendant` that has business hours configured to be 10:45 to 13:15 (Sunday), 09:00 to 17:00 (Monday), and 09:00 to 16:30 (Saturday) and holiday times and their associated greetings configured to be " `New Year`" on January 2, 2013, and " `Building Closed for Construction`" from April 24, 2013 through April 28, 2013.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessHoursSchedule 0.10:45-0.13:15,1.09:00-1.17:00,6.09:00-6.16:30 -HolidaySchedule "New Year,newyrgrt.wav,1/2/2013","Building Closed for Construction,construction.wav,4/24/2013,4/28/2013"
```

This example configures a UM auto attendant named `MyAutoAttendant` and enables non-business hours key mappings so that when callers press 1, they're forwarded to another UM auto attendant named `SalesAutoAttendant`. When they press 2, they're forwarded to extension number 12345 for `Support`, and when they press 3, they're sent to another auto attendant that plays an audio file.

```powershell
Set-UMAutoAttendant -Identity MyAutoAttendant - BusinessHoursKeyMappingEnabled $true -BusinessHoursKeyMapping "1,Sales,,SalesAutoAttendant","2,Support,12345","3,Directions,,,directions.wav"
```
