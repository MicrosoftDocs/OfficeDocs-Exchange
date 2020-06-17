---
title: 'Delete a UM auto attendant: Exchange 2013 Help'
TOCTitle: Delete a UM auto attendant
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 92846bbc-e6b9-45fc-8702-ef5c92eeb08f
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Delete a UM auto attendant in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

After you delete a Unified Messaging (UM) auto attendant, the incoming calls that were answered by the UM auto attendant must be answered by a human operator. A UM auto attendant can't be deleted if it's associated with a UM dial plan as the default UM auto attendant.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to delete a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to edit, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to delete. On the toolbar, click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif). On the **Warning** page, click **Yes**.

## Use the Shell to delete a UM auto attendant

This example deletes a UM auto attendant named `MyUMAutoAttendant`.

```powershell
Remove-UMAutoAttendant -Identity MyUMAutoAttendant
```
