---
localization_priority: Normal
description: 'Summary: How to remove a database availability group (DAG) in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 071296e9-31b0-40f4-9a02-177d97486ebd
ms.date: 7/9/2018
ms.reviewer: 
title: Remove a database availability group
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Remove a database availability group

Removing a DAG is a quick and easy task. You can use the EAC or the Exchange Management Shell to remove a DAG.

Looking for other management tasks related to DAGs? Check out [Manage database availability groups](manage-dags.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- Before you can remove a DAG, the DAG must be empty. If the DAG you want to remove contains any Mailbox servers, you must first remove the servers from the DAG. For detailed steps about how to remove a Mailbox server from a DAG, see [Manage database availability group membership](dag-memberships.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351)..

## Use the EAC to remove a database availability group

1. Navigate to **Servers** \> **Database availability groups**.

2. Select the DAG you want to remove and click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.png).

3. Click **Yes** to confirm the warning and remove the DAG.

## Use the Exchange Management Shell to remove a database availability group

This example removes the DAG DAG1.

```
Remove-DatabaseAvailabilityGroup -Identity DAG1
```

## How do you know this worked?

To verify that you've successfully removed the DAG, do one of the following:

- In the EAC, go to **Servers** \> **Database Availability Groups**, and see if the DAG is still displayed.

- In the Exchange Management Shell, run the following command to see if the DAG still exists:

  ```
  Get-DatabaseAvailabilityGroup <DAGName>
  ```

    If the DAG was successfully deleted, the preceding command will produce an error message indicating the object could not be found.
