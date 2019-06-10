---
title: 'Install Exchange 2013 using unattended mode: Exchange 2013 Help'
TOCTitle: Install Exchange 2013 using unattended mode
ms:assetid: 386465e9-41da-4e26-9816-b3b69be1f8bf
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Aa997281(v=EXCHG.150)
ms:contentKeyID: 48384985
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Install Exchange 2013 using unattended mode

_**Applies to:** Exchange Server 2013_

To perform an unattended setup, you must install Microsoft Exchange Server 2013 from the command prompt. For more information about planning and deploying Exchange 2013, see [Planning and deployment](planning-and-deployment-for-exchange-2013-installation-instructions.md).

We recommend that the Edge Transport role be installed in a perimeter network outside of your organization's internal Active Directory forest. While you can install the Edge Transport server role on a domain-joined computer, doing so will only enable domain management of Windows features and settings. The Edge Transport role itself doesn't use Active Directory. Instead, it uses the Active Directory Lightweight Directory Services (AD LDS) Windows feature to store configuration and recipient information. For more information about the Edge Transport role, see [Edge Transport servers](edge-transport-servers-exchange-2013-help.md).

> [!TIP]
> Have you heard about the Exchange Server Deployment Assistant? It's a free online tool that helps you quickly deploy Exchange 2013 in your organization by asking you a few questions and creating a customized deployment checklist just for you. If you want to learn more about it, go to <A href="exchange-server-deployment-assistant-exchange-2013-help.md">Exchange Server Deployment Assistant</A>.

> [!NOTE]
> After you install any server roles on a computer running Exchange 2013, you can't use the Exchange 2013 Setup wizard to add any additional server roles to this computer. If you want to add more server roles to a computer, you must either use Add or Remove Programs from Control Panel or use Setup.exe from a Command Prompt window.<BR>The Edge Transport role can't be installed on the same computer as the Mailbox or Client Access server roles.

For information about tasks to complete after installation, see [Exchange 2013 post-Installation tasks](exchange-2013-post-installation-tasks-exchange-2013-help.md).

## What do you need to know before you begin?

The following information applies to all Exchange 2013 server roles.

- Make sure you've read the release notes prior to installing Exchange 2013. For more information, see [Release notes for Exchange 2013](release-notes-for-exchange-2013-exchange-2013-help.md).

- The computer you install Exchange 2013 on must have a supported operating system (such as Windows Server 2008 R2 with Service Pack 1 (SP1), Windows Server 2012 R2, or Windows Server 2012), have enough disk space, and satisfy other requirements. For information about system requirements, see [Exchange 2013 system requirements](exchange-2013-system-requirements-exchange-2013-help.md).

- To run Exchange 2013 setup, you must install Microsoft .NET Framework 4.5, Windows Management Framework, and other required software. To understand the prerequisites for all server roles, see [Exchange 2013 prerequisites](exchange-2013-prerequisites-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!WARNING]
> After you install Exchange on a server, you must not change the server name. Renaming a server after you have installed an Exchange server role is not supported.

The following information applies to the Exchange 2013 Mailbox and Client Access server roles.

- Estimated time to complete: 60 minutes

- Each organization requires at a minimum one Client Access server and one Mailbox server in the Active Directory forest. Additionally, each Active Directory site that contains a Mailbox server must also contain at least one Client Access server. If you're separating your server roles, we recommend installing the Mailbox server role first.

- The computer you install Exchange 2013 on must be a member of an Active Directory domain.

- You must ensure the account you use is delegated membership in the Schema Admins group if you haven't previously prepared the Active Directory schema. If you're installing the first Exchange 2013 server in the organization, the account you use must have membership in the Enterprise Admins group. If you've already prepared the schema and aren't installing the first Exchange 2013 server in the organization, the account you use must be a member of the Exchange 2013 Organization Management management role group.

    Administrators who are members of the Delegated Setup role group can deploy Exchange 2013 servers that have been previously provisioned by a member of the Organization Management role group.

The following information applies to the Exchange 2013 Edge Transport server role.

- Estimated time to complete: 40 minutes

- The Edge Transport role is available with Exchange 2013 SP1 or later.

- You need to configure the primary DNS suffix on the computer. For example, if the fully qualified domain name of your computer is edge.contoso.com, the DNS suffix for the computer is contoso.com. For more information, see [Primary DNS Suffix is missing](primary-dns-suffix-is-missing-exchange-2013-help.md).

- Exchange 2007 and Exchange 2010 Hub Transport servers need an update before you can create an EdgeSync Subscription between them and an Exchange 2013 Edge Transport server. If you don't install this update, the EdgeSync Subscription won't work correctly. For more information, see the "Supported coexistence scenarios" section in [Exchange 2013 system requirements](exchange-2013-system-requirements-exchange-2013-help.md).

- Make sure the account you use is a member of the local Administrators group on the computer you're installing the Edge Transport role.

## Use Setup.exe to install Exchange 2013 in unattended mode

> [!NOTE]
> To download the latest version of Exchange 2013, see <A href="updates-for-exchange-2013-exchange-2013-help.md">Updates for Exchange 2013</A>.

1. Log on to the computer on which you want to install Exchange 2013.

2. Navigate to the network location of the Exchange 2013 installation files.

3. At the command prompt, run the applicable command for your organization.

    > [!IMPORTANT]
    > If you have User Access Control (UAC) enabled, you must run <CODE>Setup.exe</CODE> from an elevated command prompt.

    ```powershell
    Setup.exe [/Mode:<setup mode>] [/IAcceptExchangeServerLicenseTerms]
    [/Roles:<server roles to install>] [/InstallWindowsComponents]
    [/OrganizationName:<name for the new Exchange organization>]
    [/TargetDir:<target directory>] [/SourceDir:<source directory>]
    [/UpdatesDir:<directory from which to install updates>]
    [/DomainController:<FQDN of domain controller>] [/DisableAMFiltering]
    [/AnswerFile:<filename>] [/DoNotStartTransport]
    [/EnableErrorReporting] [/CustomerFeedbackEnabled:<True | False>]
    [/AddUmLanguagePack:<UM language pack name>]
    [/RemoveUmLanguagePack:<UM language pack name>]
    [/NewProvisionedServer:<server>] [/RemoveProvisionedServer:<server>]
    [/MdbName:<mailbox database name>] [/DbFilePath:<Edb file path>]
    [/LogFolderPath:<log folder path>] [/ActiveDirectorySplitPermissions:<True | False>]
    [/TenantOrganizationConfig:<path>]
    ```

4. Setup copies the setup files locally to the computer on which you're installing Exchange 2013.

5. Setup checks the prerequisites, including all prerequisites specific to the server roles that you're installing. If you haven't met all the prerequisites, Setup fails and returns an error message that explains the reason for the failure. If you've met all the prerequisites, Setup installs Exchange 2013.

6. Restart the computer after Exchange 2013 has completed.

7. Complete your deployment by performing the tasks provided in [Exchange 2013 post-Installation tasks](exchange-2013-post-installation-tasks-exchange-2013-help.md).

## Examples

The following are examples of using Setup.exe:

- **Setup.exe /mode:Install /role:ClientAccess,Mailbox /OrganizationName:MyOrg /IAcceptExchangeServerLicenseTerms**

    This command creates an Exchange 2013 organization in Active Directory called MyOrg, installs the Client Access server role, Mailbox server role, and the management tools and also accepts the Exchange 2013 licensing terms.

- **Setup.exe /mode:Install /role:ClientAccess,Mailbox /TargetDir:"C:\\Exchange Server" /IAcceptExchangeServerLicenseTerms**

    This command installs the Client Access server role, the Mailbox server role, and the management tools to the "C:\\Exchange Server" directory. This command assumes an Exchange 2013 organization has already been prepared.

- **Setup.exe /mode:Install /r:CA,MB /IAcceptExchangeServerLicenseTerms**

    This command installs the Client Access server role, the Mailbox server role, and the management tools to the default installation location.

- **Setup.exe /mode:Install /r:EdgeTransport /IAcceptExchangeServerLicenseTerms**

    This command installs the Edge Transport server role and the management tools to the default installation location.

- **Setup.exe /mode:Install /r:ET /IAcceptExchangeServerLicenseTerms**

    This command installs the Edge Transport server role and the management tools to the default installation location.

- **Setup.exe /mode:Uninstall /IAcceptExchangeServerLicenseTerms**

    This command completely removes Exchange 2013 from the server and removes this server's Exchange configuration from Active Directory.

- **Setup.exe /PrepareAD /on:"My Org" /IAcceptExchangeServerLicenseTerms**

    This command creates an Exchange organization called My Org and prepares Active Directory for Exchange 2013.

- **C:\\ExchangeServer\\bin\\Setup.exe /m:Install /r:ClientAccess /SourceDir:d:\\amd64 /IAcceptExchangeServerLicenseTerms**

    This command adds the Client Access server role to an existing Exchange 2013 server using D:\\amd64 as the source directory.

- **Setup.exe /role:ClientAccess,Mailbox /UpdatesDir:"C:\\ExchangeServer\\New Patches" /IAcceptExchangeServerLicenseTerms**

    This command updates ExchangeServer.msi with patches from the specified directory, and then installs the Client Access server role, Mailbox server role, and the management tools. If a language pack bundle is included in this directory, the language pack is also installed.

- **Setup.exe /mode:Install /role:ClientAccess,Mailbox /DomainController:DC01 /IAcceptExchangeServerLicenseTerms**

    This command uses the domain controller DC01 to query and make changes to Active Directory while installing the Client Access server role, Mailbox server role, and the management tools.

- **Setup.exe /mode:Install /role:ClientAccess /AnswerFile:c:\\ExchangeConfig.txt /IAcceptExchangeServerLicenseTerms**

    This command installs the Client Access server role by using the settings in the ExchangeConfig.txt file.

- **Setup.exe /rprs:Exchange03 /IAcceptExchangeServerLicenseTerms**

    This command removes the object Exchange03 from Active Directory.

- **Setup.exe /AddUmLanguagePack:ko-KR /IAcceptExchangeServerLicenseTerms**

    This command installs the Korean Unified Messaging language pack from the %ExchangeSourceDir%\\ServerRoles\\UnifiedMessaging directory.

## How do you know this worked?

To verify that you've successfully installed Exchange 2013, see [Verify an Exchange 2013 installation](verify-an-exchange-2013-installation-exchange-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.
