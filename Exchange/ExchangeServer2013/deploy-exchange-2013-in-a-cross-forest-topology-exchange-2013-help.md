---
title: 'Deploy Exchange 2013 in a cross-forest topology: Exchange 2013 Help'
TOCTitle: Deploy Exchange 2013 in a cross-forest topology
ms:assetid: 65be650f-d435-4f60-9ff0-5cb88a726abb
ms:mtpsurl: https://technet.microsoft.com/library/Aa998597(v=EXCHG.150)
ms:contentKeyID: 50406264
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Deploy Exchange 2013 in a cross-forest topology

_**Applies to:** Exchange Server 2013_

This topic explains how to deploy Exchange 2013 in a cross-forest topology using Microsoft Forefront Identity Manager 2010 R2 SP1. To deploy Exchange 2013 in a cross-forest topology, you must first install Exchange 2013 in each forest, and then connect the forests so that users can see address and availability data across the forests.

The following figure illustrates user synchronization between two Exchange 2013 forests.

**Example of Exchange 2013 cross-forest synchronization**

![Example of Exchange 2010 multiple forest](images/Aa998597.df0ba5dd-cb96-4542-98bd-2a425defe317(EXCHG.150).gif "Example of Exchange 2010 multiple forest")

This topic does *not* describe how to deploy Exchange 2013 in a dedicated Exchange forest (or resource forest) topology. For more information about how to deploy Exchange 2013 in a resource forest topology, see [Deploy Exchange 2013 in an Exchange resource forest topology](deploy-exchange-2013-in-an-exchange-resource-forest-topology-exchange-2013-help.md).

## What do you need to know before you begin?

To perform the following procedure in Exchange 2013, confirm the following:

- You have correctly configured Domain Name System (DNS) for name resolution across forests in your organization. To verify that DNS is configured correctly, use the Ping tool to test connectivity to each forest from the other forests in your organization and from the server on which you will run the GALSync agent.

- The GALSync management agent (MA) communicates with the Exchange 2013 forest using Windows PowerShell V2.0 RTM. Make sure Windows PowerShell v1.0 isn't installed on this computer by going to Control Panel, and then clicking Programs and Features.

- Ensure that Windows Remote Management has not been installed by Windows Update.

- Install Windows PowerShell and Windows Remote Management. For details, see Microsoft Knowledge Base article 968930, [Windows Management Framework Core package (Windows PowerShell 2.0 and WinRM 2.0)](https://support.microsoft.com/help/968930).

- Download Forefront Identity Manager 2010 R2 SP1. See [Download of Microsoft Forefront Identity Manager 2010 R2 SP1](https://www.microsoft.com/evalcenter/evaluate-microsoft-identity-manager-2016/).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Deploy Exchange 2013 in a cross-forest topology with Forefront Identity Manager 2010 R2 SP1

1. In each forest, install Exchange 2013 separately. To install Exchange 2013, perform the same steps that you would if you were installing Exchange 2013 in a single forest topology. For detailed steps, see one of the following topics:

   - [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md)

   - [Install Exchange 2013 using the Setup wizard](install-exchange-2013-using-the-setup-wizard-exchange-2013-help.md)

     > [!NOTE]
     > This topic assumes that you don't have an existing Exchange 2007 or Exchange 2010 topology. If you do have an existing Exchange topology and you want to upgrade, see <A href="upgrade-from-exchange-2010-to-exchange-2013-exchange-2013-help.md">Upgrade from Exchange 2010 to Exchange 2013</A> or <A href="upgrade-from-exchange-2007-to-exchange-2013-exchange-2013-help.md">Upgrade from Exchange 2007 to Exchange 2013</A>.

2. In each forest, use Active Directory Users and Computers to create a container in which FIM 2010 R2 SP1 will create contacts for each mailbox from the other forest. We recommend that you name this container **FromFIM**. To create the container, select the domain in which you want to create the container, right-click the domain, select **New** \> **Organizational Unit**. In **New Object - Organizational Unit**, type **FromFIM**, and then click **OK**.

3. Create a GALSync management agent for each forest by using Forefront Identify Manager. This allows you to synchronize the users in each forest and create a common GAL. For detailed steps, see the following resources:

   - [Configuring Global Address List (GAL) Synchronization with Forefront Identity Manager (FIM) 2010](https://go.microsoft.com/fwlink/p/?linkid=279869)

   - [Work with Management Agents](https://go.microsoft.com/fwlink/p/?linkid=279870)

   - [Forefront Identity Manager 2010 R2 Documentation Roadmap](https://go.microsoft.com/fwlink/p/?linkid=279871)

   > [!IMPORTANT]
   > While the resources discuss Exchange 2010, Exchange 2013 is supported for FIM 2010 R2 SP1. Make sure that you configure <STRONG>Extensions</STRONG> in FIM 2010 R2 SP1 for Exchange 2013.

   On the **Configure Extensions** page, under **Configure partition display name(s)**, next to **Provision for**, select **Exchange 2013**. You will see the **Exchange 2013 RPS URI** field. Enter the URI of an Exchange 2013 Client Access server to make sure the remote PowerShell connection is functioning. The **Exchange 2013 RPS URI** should be in the following format: http://CAS_Server_FQDN/Powershell. Click **OK**.

   > [!NOTE]
   > Make sure that the administrator credentials used to connect to the Exchange 2013 forest can also make remote PowerShell connections to that forest.<BR>The following figure shows how to select provisioning for Exchange 2013.

   **Provision GalSync Management Agent for Exchange 2013**

   ![Management Agent Exchange 2010 provisioning](images/Aa998597.8f403cda-e5e4-4edf-887f-c1ed46cee3f5(EXCHG.150).gif "Management Agent Exchange 2010  provisioning")

4. Create an SMTP Send connector in each of the forests. For detailed steps, see [Configure a cross-forest Send connector](configure-a-cross-forest-send-connector-exchange-2013-help.md).

5. In each forest, enable the Availability service so that users in each forest can view free/busy data about users in the other forest. For more information, see [Availability service in Exchange 2013](availability-service-in-exchange-2013-exchange-2013-help.md).

6. If you want mail relayed through any forest in your organization, you must configure a domain in that forest as an authoritative domain. For detailed steps, see [Configure Exchange to accept mail for multiple authoritative domains](configure-exchange-to-accept-mail-for-multiple-authoritative-domains-exchange-2013-help.md).
