---
localization_priority: Normal
description: 'Summary: Pre-stage and provision a cluster name object (CNO) for an Exchange Server database availability group (DAG).'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 51ebf2f6-8a02-44ef-a489-ca361cb0f63a
ms.date: 7/9/2018
ms.reviewer: 
title: Pre-stage the cluster name object for a database availability group
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Pre-stage the cluster name object for a database availability group

In environments where computer account creation is restricted, or where computer accounts are created in a container other than the default computers container, you can pre-stage the cluster name object (CNO) and then provision the CNO by assigning permissions to it.

Pre-staging the CNO is also required for Windows Server 2012 and Windows Server 2012 R2 DAG members due to permissions changes in Windows for computer objects. When deploying a database availability group (DAG) using Mailbox servers that are running Windows Server 2012 or Windows Server 2012 R2, you must pre-stage and provision the CNO, unless you're deploying a DAG without a cluster administrative access point. DAGs without cluster administrative access points do not use CNOs; therefore pre-staging is not required for those DAGs.

You create and disable a computer account for the CNO, and then either:

- Assign full control of the computer account to the computer account of the first Mailbox server you're adding to the DAG.

    **-Or-**

- Assign full control of the computer account to the Exchange Trusted Subsystem universal security group (USG).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You must use an account that has permissions to create computer objects in Active Directory.

- After completing the following steps, allow time for Active Directory replication to occur. After the object is replicated, you can add the first member to the DAG.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351)..

## Pre-stage the CNO

1. Open Active Directory Users and Computers.

2. Expand the forest node.

3. Right-click the organizational unit (OU) in which you want to create the new account, select **New**, and then select **Computer**.

4. In **New Object - Computer**, type the computer account name for the CNO in the **Computer name** box. This is the name that you'll use for the DAG. Click **OK** to create the account.

5. Right-click the new computer account, and then click **Disable Account**. Click **Yes** to confirm the disable action, and then click **OK**.

## Assign permissions to the CNO

1. Open Active Directory Users and Computers.

2. If Advanced Features aren't enabled, turn them on by clicking **View**, and then clicking **Advanced Features**.

3. Right-click the new computer account, and then click **Properties**.

4. In **\<Computer Name\> Properties**, on the **Security** tab, click **Add** to add either the computer account for the first node to be added to the DAG or to add the Exchange Trusted Subsystem USG:

  - To add the Exchange Trusted Subsystem, type Exchange Trusted Subsystem in the **Enter the object names to select** field. Click **OK** to add the USG. Select the Exchange Trusted Subsystem USG and in the **Permissions for Exchange Trusted Subsystem** field, select **Full Control** in the **Allow** column. Click **OK** to save the permission settings.

  - To add the computer account for the first node to be added to the DAG, click **Object Types**. In the **Object Types** dialog box, clear the **Built-in security principals**, **Groups**, and **Users** check boxes. Select the **Computers** check box and click **OK**. In the **Enter the object names to select** field, type the name of the first Mailbox server to be added to the DAG, and then click **OK**. Select the first node's computer account, and in the **Permissions for \<NodeName\>** field, select **Full Control** in the **Allow** column. Click **OK** to save the permission settings.

## How do you know this worked?

To verify that you've successfully created the CNO, do the following:

1. Open Active Directory Users and Computers.

2. Expand the forest node.

3. Open the OU in which you created the account, and then verify that the account is listed.
