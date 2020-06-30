---
title: 'Configure the DNS suffix search list for a disjoint namespace'
TOCTitle: Configure the DNS suffix search list for a disjoint namespace
ms:assetid: cfa715ac-7b69-47c3-b206-933ec2cf677b
ms:mtpsurl: https://technet.microsoft.com/library/Bb847901(v=EXCHG.150)
ms:contentKeyID: 49289414
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the DNS suffix search list for a disjoint namespace

_**Applies to:** Exchange Server 2013_

This topic explains how to use the Group Policy Management console (GPMC) to configure the Domain Name System (DNS) suffix search list. In some Microsoft Exchange 2013 scenarios, if you have a disjoint namespace, you must configure the DNS suffix search list to include multiple DNS suffixes.

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes

- To perform this procedure, the account you use must be delegated membership in the Domain Admins group.

- Confirm that you have installed .NET Framework 3.0 on the computer on which you will install the GPMC.

  > [!NOTE]
  > The current version of the GPMC that you can download from the Microsoft Download Center operates on the 32-bit versions of the Windows Server 2003 and Windows XP operating systems and can remotely manage Group Policy objects on 32-bit and 64-bit domain controllers. This version of the GPMC doesn't include a 64-bit version, and the 32-bit version doesn't run on 64-bit platforms. The 32-bit version of Windows Server 2008 and the 32-bit version of Windows Vista both include a 32-bit version of the GPMC. The 64-bit version of Windows Server 2008 and the 64-bit version of Windows Vista both include a 64-bit version of the GPMC.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the GPMC to configure the DNS suffix search list

1. On a 32-bit computer in your domain, install GPMC with Service Pack 1 (SP1). For download information, see [Group Policy Management Console with Service Pack 1](https://www.microsoft.com/download/details.aspx?id=21895).

    > [!NOTE]
    > If you have a computer in your domain running Windows Server 2008 or Windows Vista, you can skip this step.

2. Click **Start** \> **Programs** \> **Administrative Tools** \> **Group Policy Management**.

3. In **Group Policy Management**, expand the forest and the domain in which you will apply Group Policy. Right-click **Group Policy Objects**, and then click **New**.

4. In **New GPO**, type a name for the policy, and then click **OK**.

5. Right-click the new policy that you created in Step 4, and then click **Edit**.

6. In **Group Policy Management Editor**, expand **Computer Configuration**, expand **Policies**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.

7. Right-click **DNS Suffix Search List**, click **All Tasks**, and then click **Edit**.

8. On the **DNS Suffix Search List Properties** page, select **Enabled**. In the **DNS Suffixes** box, type the primary DNS suffix of the disjoint computer, the DNS domain name, and any additional namespaces for other servers with which Exchange may interoperate, such as monitoring servers or servers for third-party applications. Click **OK**.

9. In **Group Policy Management**, expand **Group Policy Objects**, and then select the policy that you created in Step 4. On the **Scope** tab, scope the policy so that it applies to only the computers that are disjoint.

## How do you know this worked?

After you install Exchange 2013, verify that you can send email messages inside and outside your organization.

## For more information

[Windows Server Group Policy](https://go.microsoft.com/fwlink/p/?linkid=100128)

[Group Policy](https://go.microsoft.com/fwlink/?linkid=268043)

[Disjoint namespace scenarios](disjoint-namespace-scenarios-exchange-2013-help.md)
