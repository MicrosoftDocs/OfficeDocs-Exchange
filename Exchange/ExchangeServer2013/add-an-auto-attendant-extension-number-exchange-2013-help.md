---
title: 'Add an auto attendant extension number: Exchange 2013 Help'
TOCTitle: Add an auto attendant extension number
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: f2bd62ba-1e01-4cb7-862c-c750752e20e0
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Add an auto attendant extension number in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can configure an extension number or multiple extension numbers on a Unified Messaging (UM) auto attendant. When you add an extension number to a UM auto attendant, that number can be used by callers to call into the auto attendant. Also, you may have to add extension numbers because there is more than one extension number that callers can use to access an auto attendant. By default, no extension numbers are configured when you create an auto attendant.

You can create a new auto attendant without setting up an extension number for the auto attendant. You can also associate more than one telephone or extension number with a single auto attendant. You can either add the extension numbers when you create the UM auto attendant or add them after you configure the auto attendant. The number of digits in the extension number you configured on the UM auto attendant must match the number of digits for an extension number that's configured on the UM dial plan associated with the UM auto attendant.

> [!NOTE]
> You can also add a Session Initiation Protocol (SIP) address instead of adding an extension number. A SIP address is used by some IP Private Branch eXchanges (PBXs) and Office Communications Server 2007 R2 or Microsoft Lync Server.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to add an extension or phone numbers for a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to edit and click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to add extension or phone numbers to.

3. On the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

4. On the **UM Auto Attendant** page \> **General**, under **Access numbers**, in the text box, enter the extension or phone number that you want to use and click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

5. Click **Save** to add the number.

## Use the Shell to configure an extension number on a UM auto attendant

This example configures a UM auto attendant named `MyUMAutoAttendant` with multiple extension numbers.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -PilotIdentifierList "12345","72000","75000"
```
