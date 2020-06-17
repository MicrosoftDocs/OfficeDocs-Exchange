---
localization_priority: Priority
description: 'Summary: Guidance for planning and deploying Exchange 2016 or Exchange 2019.'
ms.topic: hub-page
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 692c59e3-f0b0-4cef-a66e-751aa740abae
ms.reviewer: 
title: Planning and deployment for Exchange Server
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Planning and deployment for Exchange Server

This topic contains links to topics and information about planning for and then deploying Exchange Server 2016 or Exchange Server 2019.

> [!IMPORTANT]
> Make sure you read the [Release notes for Exchange Server](../release-notes.md) topics before you begin your deployment. The release notes contain important information on issues you might encounter during and after your deployment.

## Plan for Exchange Server

 Use the information at following links to help plan your deployment of Exchange 2016 or Exchange 2019 into your organization.

> [!IMPORTANT]
> See the [Establish an Exchange 2016 or Exchange 2019 test environment](#establish-an-exchange-2016-or-exchange-2019-test-environment) section later in this topic for information about installing Exchange 2019 in a test environment.

[Exchange architecture](../architecture/architecture.md)

> Learn about the Mailbox and Edge Transport server roles and more in Exchange.

[Exchange Server system requirements](system-requirements.md)

> Understand the system requirements that need to be satisfied in your organization before you can install Exchange.

[Exchange Server prerequisites](prerequisites.md)

> Learn about the Windows Server features and the other software that needs to be installed for a successful installation of Exchange.

[Microsoft Exchange Server Deployment Assistant](https://assistants.microsoft.com/)

> Use this tool to generate a customized checklist for planning, installing, or upgrading Exchange. Guidance is available for multiple scenarios, including an on-premises, hybrid, or cloud deployment.

[Active Directory](active-directory/active-directory.md)

> Learn about how Exchange uses Active Directory and how your Active Directory deployment affects your Exchange deployment.

[Antispam and antimalware protection in Exchange Server](../antispam-and-antimalware/antispam-and-antimalware.md)

> Learn about the built-in antispam and antimalware protection options in Exchange.

[Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid)

> Learn about planning a hybrid deployment with Microsoft Office 365 and your on-premises Exchange organization.

[Exchange Server virtualization](../plan-and-deploy/virtualization.md)

> Learn how you can deploy Exchange in a virtualized environment.

[Exchange Online and Exchange development](https://docs.microsoft.com/exchange/client-developer/exchange-server-development)

> Learn about the application programming interfaces (APIs) that are available for applications that use Exchange 2019.

### Establish an Exchange 2016 or Exchange 2019 test environment

Before you install your first Exchange server, we recommend that you install Exchange in an isolated test environment. This approach reduces the risk of end-user downtime and negative ramifications to the production environment.

The test environment will act as your "proof of concept" for your new Exchange design and make it possible to move forward or roll back any implementations before deploying into your production environments. Having an exclusive test environment for validation and testing allows you to do pre-installation checks for your future production environments. By installing in a test environment first, we believe that your organization will have a better likelihood of success in a full production implementation.

For many organizations, the costs of building a test lab may be high because of the need to duplicate the production environment. To reduce the hardware costs associated with a prototype lab, we recommend the use of virtualization by using Hyper-V technologies in Windows Server. Hyper-V enables server virtualization, allowing multiple virtual operating systems to run on a single physical machine.

For more detailed information about Hyper-V, see [Server Virtualization](https://docs.microsoft.com/windows-server/virtualization/virtualization). For information about the Microsoft support of production Exchange servers on hardware virtualization software, see [Exchange Server virtualization](virtualization.md).

## Deploy Exchange 2016 or Exchange 2019

During the deployment phase, you install Exchange into your organization. Before you begin the deployment phase, you should plan your Exchange organization. For more information, see the [Plan for Exchange Server](#plan-for-exchange-server) section earlier in this topic.

Use the information at the following links to help you deploy Exchange.

[Prepare Active Directory and domains for Exchange](prepare-ad-and-domains.md)

> Learn about the steps you need to take to prepare your Active Directory forest for Exchange 2019 and the changes Exchange makes to your forest.

[Install Exchange Mailbox servers using the Setup wizard](deploy-new-installations/install-mailbox-role.md)

> Learn about using the Setup wizard to install Mailbox servers.

[Use unattended mode in Exchange Setup](deploy-new-installations/unattended-installs.md)

> Learn about using the unattended setup at the command line to install, remove, update, and recover Exchange servers.

[Install Exchange Edge Transport servers using the Setup wizard](deploy-new-installations/install-edge-transport-role.md)

> Learn about using the Setup wizard to install Edge Transport servers in a perimeter network.

[Upgrade Exchange to the latest Cumulative Update](install-cumulative-updates.md)

> Learn about finding and installing the latest Cumulative Update (CU) for the Exchange servers in your organization.

[Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid)

> Read this topic for information that will help you deploy Exchange in an existing hybrid deployment.

[Exchange Server post-installation tasks](post-installation-tasks/post-installation-tasks.md)

> Learn about post-installation tasks to complete your Exchange installation.

## Exchange Setup

You can use different types and modes of Exchange Setup to install and maintain the various editions and versions of Exchange.

### Exchange editions and versions

Exchange is available in two server editions: Standard Edition and Enterprise Edition. The edition you install is defined by your product key (the only available download can install both versions). For more information, see [Exchange licensing FAQs](https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-server-licensing-licensing-overview).

### Types of Exchange Setup

You have the following options for Exchange Setup:

- **Exchange Setup wizard**: Running Setup.exe without any command line switches provides an interactive experience where you're guided by the Exchange 2019 Setup wizard.

- **Exchange unattended setup**: Running Setup.exe with command line switches enables you to install Exchange from an interactive command line or through a script.

### Modes of Exchange Setup

Exchange setup includes the following modes:

- **Install**: Install a new server role (Mailbox server, Edge Transport server, or Management tools). This mode is available in the Exchange Setup wizard and unattended setup.

- **Uninstall**: Remove the Exchange installation from a computer. You can use this mode from both the Exchange Setup wizard and unattended setup.

- **Upgrade**: Install a CU on an existing Exchange server. You can use this mode from both the Exchange Setup wizard and unattended setup.

  > [!NOTE]
  > Exchange doesn't support in-place upgrades from previous versions. This mode is used only to install CUs.

- **RecoverServer**: You need to recover data from the Exchange server after a catastrophic failure. To do this, you install a new Windows server with the same FQDN as the failed server (for example, mailbox01.contoso.com), and then run Exchange Setup with the _/Mode:RecoverServer_ switch without specifying the Exchange server roles to restore.

    Setup detects the Exchange server object in Active Directory and installs the corresponding files and configuration automatically. After you recover the server, you can restore databases and reconfigure any additional settings. To run in **RecoverServer** mode:

    - Exchange can't be already installed on the server.

    - The Exchange server object must exist in Active Directory.

    - You can only use unattended setup.

  > [!NOTE]
  > You must complete one mode of Setup before you can use another mode.
