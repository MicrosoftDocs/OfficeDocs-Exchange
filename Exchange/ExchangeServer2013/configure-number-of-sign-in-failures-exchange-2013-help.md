---
title: 'Configure the number of sign-in failures before Outlook Voice Access users are disconnected: Exchange 2013 Help'
TOCTitle: Configure the number of sign-in failures before Outlook Voice Access users are disconnected
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 02f93888-168c-44bb-8cf6-17f5fcc3d733
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the number of sign-in failures before Outlook Voice Access users are disconnected in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can specify the number of sequential unsuccessful sign-in attempts that are allowed before a caller is disconnected. The value of this setting can be from 1 through 20. Setting this value too low can frustrate users. For most organizations, this value should be set to the default of three attempts.

For additional management tasks related to UM dial plans, see [UM dial plan procedures in Exchange Server](um-dial-plan-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure the number of sign-in failures before users are disconnected

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Settings**, under **Number of sign-in failures before disconnecting**, enter the number of sign-in failures.

5. Click **Save**.

## Use the Shell to configure the number of sign-in failures before users are disconnected

This example sets the number of sign-in failures before users are disconnected to 5 for a UM dial plan named `MyUMDialPlan`.

```powershell
Set-UMDialPlan -identity MyUMDialPlan -LogonFailuresBeforeDisconnect 5
```
