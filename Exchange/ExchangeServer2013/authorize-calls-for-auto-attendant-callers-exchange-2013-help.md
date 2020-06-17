---
title: 'Authorize calls for auto attendant callers: Exchange 2013 Help'
TOCTitle: Authorize calls for auto attendant callers
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c6c94fad-64df-44aa-a198-980f017ef716
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Authorize calls for auto attendant callers in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable dialing authorizations on a Unified Messaging (UM) auto attendant. Dialing authorizations on an auto attendant are used to prohibit users who call in to the auto attendant from making in-country/region or international telephone calls, or outdialing. Outdialing happens when Unified Messaging makes an outgoing call for a user after they've called into a phone number that is configured on a UM auto attendant.

For additional management tasks related to outdialing, see [Allowing users to make calls procedures](allow-users-to-make-calls-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- Before you perform these procedures, confirm that in-country/region and international dialing rules have been created on a UM dial plan. For detailed steps, see [Create dialing rules for users](create-dialing-rules-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable dialing authorizations on a UM auto attendant for in-country/region rule groups

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to create a dialing authorization, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Dialing authorization**, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) under **Authorized in-country/region dialing rule groups**.

4. On the **Select Dialing Rule Groups to Allow** page, select the dialing rule group, click **OK**, and then click **Save**.

## Use the EAC to enable dialing authorizations on a UM auto attendant for international rule groups

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to create a dialing authorization, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Dialing authorization**, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) under **Authorized international dialing rule groups**.

4. On the **Select Dialing Rule Groups to Allow** page, select the dialing rule group, click **OK**, and then click **Save**.

## Use the Shell to enable in-country/region and international dialing authorizations on a UM auto attendant

This example enables the InCountry/RegionGroup1, InCountry/RegionGroup2. InternationalGroup1, and InternationalGroup2 dialing authorizations on a UM auto attendant named `MyUMAutoAttendant`.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -AllowedInCountryOrRegionGroups InCountry/RegionGroup1,InCountry/RegionGroup2 -AllowedInternationalGroups InternationalGroup1,InternationalGroup2
```
