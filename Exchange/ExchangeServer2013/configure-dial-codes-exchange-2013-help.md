---
title: 'Configure dial codes: Exchange 2013 Help'
TOCTitle: Configure dial codes
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: e5b5efee-b734-4f70-8357-11be07b23bd0
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure dial codes in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can configure dial codes, number prefixes, and number formats that are used by Unified Messaging to dial incoming and outgoing calls for users who are enabled for UM. In most cases, you'll configure a dial plan with the dial codes, prefixes, and number formats currently configured on your telephony network.

Dial codes and number prefixes are used to determine the correct number to dial for an outgoing call that's placed by a UM-enabled user. Outdialing is the term used to describe the process by which a user in a UM dial plan initiates an outgoing call. Number formats are used for incoming calls within a country or region, international calls, or calls that are placed within a dial plan. You can configure a dial plan to match the incoming call number format for both in-country/region and international numbers. When you configure the in-country/region and international number formats, you can restrict incoming calls for users linked with a dial plan.

For additional management tasks related to outdialing, see [Allowing users to make calls procedures](allow-users-to-make-calls-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure dial codes, prefixes, and number formats

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. Select the UM dial plan you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Dial Plan** page, click **Configure**.

4. On the **UM dial plan** page \> **Dial codes**, configure the following options:

   - **Outside line access code**

   - **International access code**

   - **National number prefix**

   - **Country/Region code**

5. Under **Number formats for dialing between dial plans**, configure the following:

   - **Country/Region number format**

   - **International number format**

   - **Number formats for incoming calls within the same dial plan**: To add a number format, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

6. Click **Save** to save your changes.

## Use the Shell to configure dial codes, prefixes, and number formats

This example configures a UM dial plan named `MyUMDialPlan` with an in-country or region number format, an international number format, and the following dial codes:

- 9 for the outside line access code

- 011 for the international access code

- 1 for the national number prefix

- 1 for the country or region code

```powershell
Set-UMDialPlan -Identity MyUMDialPlan -OutsideLineAccessCode 9 -InternationalAccessCode 011 -NationalNumberPrefix 1 CountryorRegionCode 1 -InCountryOrRegionNumberFormat 1425xxxxxxx -InternationalNumberFormat 441425xxxxxxx
```
