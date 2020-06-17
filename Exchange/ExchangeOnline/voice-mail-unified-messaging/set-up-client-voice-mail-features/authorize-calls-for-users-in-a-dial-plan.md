---
localization_priority: Normal
description: You can enable dialing authorizations on a Unified Messaging (UM) dial plan. Dialing authorizations on a dial plan are used to prohibit unauthenticated Outlook Voice Access users from making in-country/region or international telephone calls, or outdialing. Outdialing happens when Unified Messaging places an outgoing call for a user after they've called in to an Outlook Voice Access phone number that is configured on a UM dial plan. When you configure a setting on a UM dial plan, that setting applies to all unauthenticated users that call in to an Outlook Voice Access number.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 7c7fd0c4-4001-408e-b352-c49bac9f78cc
ms.reviewer: 
f1.keywords:
- NOCSH
title: Authorize calls for users in a dial plan in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Authorize calls for users in a dial plan in Exchange Online

You can enable dialing authorizations on a Unified Messaging (UM) dial plan. Dialing authorizations on a dial plan are used to prohibit unauthenticated Outlook Voice Access users from making in-country/region or international telephone calls, or outdialing. Outdialing happens when Unified Messaging places an outgoing call for a user after they've called in to an Outlook Voice Access phone number that is configured on a UM dial plan. When you configure a setting on a UM dial plan, that setting applies to all unauthenticated users that call in to an Outlook Voice Access number.

For additional management tasks related to outdialing, see [Allowing users to make calls procedures](allow-users-to-make-calls-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md)) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform this procedure, confirm that in-country/region and international dialing rules have been created on a UM dial plan. For detailed steps, see [Create dialing rules for users](create-dialing-rules.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable dialing authorizations on a UM dial plan for in-country/region dialing rule groups

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, click **Configure**.

3. On the **UM Dial Plan** page \> **Dialing authorization**, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) under **Authorized in-country/region dialing rule groups**.

4. On the **Select Dialing Rule Groups to Allow** page, select the dialing rule group, click **OK**, and then click **Save**.

## Use the EAC to enable dialing authorizations on a UM dial plan for international dialing rule groups

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, click **Configure**.

3. On the **UM Dial Plan** page \> **Dialing authorization**, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) under **Authorized international dialing rule groups**.

4. On the **Select Dialing Rule Groups to Allow** page, select the dialing rule group, click **OK**, and then click **Save**.

## Use Exchange Online PowerShell to enable in-country/region and international dialing authorizations on a UM dial plan

This example enables the InCountry/RegionGroup1, InCountry/RegionGroup2, InternationalGroup1, and InternationalGroup2 dialing authorizations on a UM dial plan named `MyUMDialPlan`.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -AllowedInCountryOrRegionGroups InCountry/RegionGroup1,InCountry/RegionGroup2 -AllowedInternationalGroups InternationalGroup1,InternationalGroup2
```
