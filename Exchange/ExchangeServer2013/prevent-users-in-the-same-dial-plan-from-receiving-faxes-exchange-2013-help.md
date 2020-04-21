---
title: 'Prevent users in the same dial plan from receiving faxes: Exchange 2013 Help'
TOCTitle: Prevent users in the same dial plan from receiving faxes
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 4fc66414-c950-4bca-ac20-4e489f288d06
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Prevent users in the same dial plan from receiving faxes in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can prevent UM-enabled users who are linked with a Unified Messaging (UM) dial plan from receiving fax messages. By default, users who are enabled for Unified Messaging and are linked with a UM dial plan can receive fax messages. However, there may be times when you want to prevent users who are associated with a specific UM dial plan from receiving faxes.

You can prevent UM-enabled users from receiving faxes by configuring the UM dial plan, the UM mailbox policy, or the UM-enabled user's mailbox. If you disable incoming fax message delivery on a UM dial plan, all users who are associated with the dial plan will be prevented from receiving fax messages. Enabling or disabling faxing on a UM dial plan takes precedence over the settings for an individual UM-enabled user.

> [!NOTE]
> You can use the EAC to configure fax settings on a UM mailbox policy. However, you must use the Shell to configure fax settings on dial plans or for individual users.

For additional management tasks related to faxing, see [Faxing procedures](faxing-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging Permissions](https://technet.microsoft.com/library/d326c3bc-8f33-434a-bf02-a83cc26a5498.aspx) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to prevent users who are linked to a dial plan from receiving faxes

This example prevents UM-enabled users associated with the UM dial plan named `MyUMDialPlan` from receiving faxes.

```powershell
Set-UMDialPlan -Identity MyUMDialPlan -FaxEnabled $false
```
