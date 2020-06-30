---
localization_priority: Priority
description: 'Summary: Learn how to install, uninstall, upgrade, and recover Exchange 2016 or Exchange 2019 from the command line.'
ms.topic: get-started-article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 386465e9-41da-4e26-9816-b3b69be1f8bf
ms.reviewer: 
title: Use unattended mode in Exchange Setup
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Use unattended mode in Exchange Setup

Running Exchange Setup from the command line allows you to automate the installation of Exchange do and other related tasks on Exchange servers (for example, remove an existing Exchange server or recover a failed Exchange server).

This topic describes the available command line switches, and provides examples.

For more information about planning for Exchange 2016 or Exchange 2019, see [Planning and deployment for Exchange Server](../../plan-and-deploy/plan-and-deploy.md).

For information about tasks to complete after installation, see [Exchange Server post-installation tasks](../post-installation-tasks/post-installation-tasks.md).

## Primary command line switches for unattended mode

The primary (top-level, scenario-defining) command line switches that are available in unattended Setup mode in Exchange 2016 or Exchange 2019 are described in the following table:

|**Switch**|**Description**|
|:-----|:-----|
|_/IAcceptExchangeServerLicenseTerms_|This switch is required in all unattended setup commands (whenever you run Setup.exe with any additional switches). If you don't use this switch, you'll get an error. To read the license terms, visit [Microsoft License Terms](https://www.microsoft.com/useterms).|
|_/Mode:\<InstallationMode\>_ <br/> (or _/m:\<InstallationMode\>_)|Valid values are: <br/>• **Install**: Installs Exchange on a new server using the Exchange server roles specified by the _/Roles_ switch. This is the default value if the command doesn't use the _/Mode_ switch. <br/>• **Uninstall**: Uninstalls Exchange from a working server. <br/>• **Upgrade**: Installs a Cumulative Update (CU) on an Exchange server. <br/>• **RecoverServer**: Recovers an Exchange server using the existing Exchange server object in Active Directory after a catastrophic hardware or software failure on the server. For instructions, see [Recover Exchange servers](../../high-availability/disaster-recovery/recover-exchange-servers.md).|
|_/Roles:\<ServerRole\>_ <br/> (or _/Role:\<ServerRole\>_ or _/r:\<ServerRole\>_)|This switch is required in `/Mode:Install` commands. Valid values are: <br/>• **Mailbox (or mb)**: Installs the Mailbox server role and the Exchange management tools on the local server. This is the default value. You can't use this value with **EdgeTransport**. <br/>• **EdgeTransport (or et)**: Installs the Edge Transport server role and the Exchange management tools on the local server. You can't use this value with **Mailbox**. <br/>• **ManagementTools (or mt or t)**: Installs the Exchange management tools on clients or other Windows servers that aren't running Exchange.|
|_/PrepareAD_ (or _/p_) <br/> _/PrepareSchema_ (or _/ps_) <br/> _/PrepareDomain:\<DomainFQDN\>_ (or _/pd:\<DomainFQDN\>_) <br/> _/PrepareAllDomains_ (or _/pad_)|Use these switches to extend the Active Directory schema for Exchange, prepare Active Directory for Exchange, and prepare some or all Active Directory domains for Exchange. For more information, see [Prepare Active Directory and domains for Exchange](../prepare-ad-and-domains.md)|
|_/NewProvisionedServer[:\<ServerName\>]_ (or _/nprs[:\<ServerName\>]_ <br/> _/RemoveProvisionedServer:\<ServerName\>_ (or _/rprs:\<ServerName\>_)|The _/NewProvisionedServer_ switch creates the Exchange server object in Active Directory. After that, a member of the Delegated Setup role group can install Exchange on the server. For more information, see [Delegate the installation of Exchange servers](delegate-installations.md). <br/><br/> The _/RemoveProvisionedServer_ switch removes a provisioned Exchange server object from Active Directory _before_ Exchange is installed on the server.|
|_/AddUmLanguagePack:\<Culture1\>,\<Culture2\>...\<CultureN\>_ <br/> _/RemoveUmLanguagePack:\<Culture1\>,\<Culture2\>...\<CultureN\>_|**Note**: These switches aren't available in Exchange 2019. They're only available in Exchange 2016. <br/><br/> Adds or removes Unified Messaging (UM) language packs from existing Exchange 2016 Mailbox servers. UM language packs enable callers and Outlook Voice Access users to interact with the UM system in those languages. You can't add or remove the en-US language pack. <br/> You can install language packs on existing Mailbox servers by using the _/AddUmLanguagePack_ switch or by running the UMLanguagePack.\<Culture\>.exe file directly. You can only remove installed language packs by using the _/RemoveUmLanguagePack_ switch. For more information, see [UM languages, prompts, and greetings](https://docs.microsoft.com/exchange/um-languages-prompts-and-greetings-exchange-2013-help).|

## Optional command line switches for unattended mode

The optional (supporting) command line switches that are available in unattended Setup mode in Exchange 2016 or Exchange 2019 are described in the following table:

|**Switch**|**Valid values**|**Default value**|**Available with|Description**|
|:-----|:-----|:-----|:-----|:-----|
|_/ActiveDirectorySplitPermissions:\<TrueOrFalse\>_|True or False|False|`/Mode:Install /Roles:Mailbox` or _/PrepareAD_ commands for the first Exchange server in the organization.|Specifies the Active Directory split permissions model when preparing Active Directory. For more information, see the "Active Directory split permissions" section in [Understanding split permissions](https://docs.microsoft.com/exchange/understanding-split-permissions-exchange-2013-help).|
|_/AdamLdapPort:\<TCPPortNumber\>_|A valid TCP port number|50389|`/Mode:Install /Roles:EdgeTransport` commands|Specifies a custom LDAP port to use for the Active Directory Lightweight Directory Services (AD LDS) instance on Edge Transport servers. The value is stored in the registry at `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ExchangeServer\v15\EdgeTransportRole\AdamSettings\MSExchange\LdapPort`.|
|_/AdamSslPort:\<TCPPortNumber\>_|A valid TCP port number|50636|`/Mode:Install /Roles:EdgeTransport` commands|Specifies a custom SSL (TLS) port to use for the AD LDS instance on Edge Transport servers. The value is stored in the registry at `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ExchangeServer\v15\EdgeTransportRole\AdamSettings\MSExchange\SslPort`.|
|_/AnswerFile:"\<PathAndFileName\>"_ <br/> (or _af:"\<PathAndFileName\>"_)|The name and location of a text file (for example,"D:\Server data\answer.txt").|n/a|`/Mode:Install /Roles:Mailbox`  or `/Mode:Install /Roles:EdgeTransport` commands|Use this switch to create a text file that you can use to install Exchange on multiple computers with the same settings. You can use the following switches in the answer file: _AdamLdapPort_, _AdamSslPort_, _CustomerFeedbackEnabled_, _DbFilePath_, _DisableAMFiltering_, _DoNotStartTransport_, _EnableErrorReporting_, _IAcceptExchangeServerLicenseTerms_, _LogFolderPath_, _Mdbname_, _OrganizationName_, _TenantOrganizationConfig_, and _UpdatesDir_. Don't use the forward slash character ( / ) with the switches in the answer file. Put each switch or switch/value pair on one line in the file.|
|_/CustomerFeedbackEnabled:\<TrueOrFalse\>_|True or False|False|`/Mode:Install` and _/PrepareAD_ commands|Specifies whether to allow or prevent Exchange from providing usage feedback to Microsoft to help improve future Exchange features. You can enable or disable error reporting on the server after setup is complete by using the _ErrorReportingEnabled_ parameter on the **Set-ExchangeServer** cmdlet.|
|_/DbFilePath:"\<Path\>\\\<FileName\>.edb"_|A folder path and an .edb filename (for example, "D:\Exchange Database Files\DB01\db01.edb").|**%ExchangeInstallPath%Mailbox\\<DatabaseName\>\\<DatabaseName\>.edb** where: <br/>• \<DatabaseName> is **Mailbox Database \<10DigitNumber\>** that matches the default name of the database **or** the value you specified with the _/MdbName_ switch (without the .edb file name extension). <br/>• %ExchangeInstallPath% is **%ProgramFiles%\Microsoft\Exchange Server\V15\\** or the location you specified with the _/TargetDir_ switch.|`/Mode:Install /Roles:Mailbox` commands|Specifies the location of the first mailbox database that's created on the new Mailbox server. You can specify the name of the database file with the _/MdbName_ switch and the location of the database transaction log files with the _/LogFolderPath_ switch.|
|_/DisableAMFiltering_|n/a|n/a|`/Mode:Install /Roles:Mailbox` commands|Disables the built-in Exchange antimalware filtering on Mailbox servers. For more information about antimalware filtering, see [Antimalware protection in Exchange Server](../../antispam-and-antimalware/antimalware-protection/antimalware-protection.md).|
|_/DomainController:\<ServerNameOrFQDN\>_ <br/> (or _/dc:\<ServerNameOrFQDN\>_)|The server name (for example, DC01) or FQDN (for example, dc01.contoso.com) of the domain controller.|A randomly-selected domain controller in the same Active Directory site as the target server where you're running Setup.|All _/Mode_ commands (except when you're installing an Edge Transport server) or _/PrepareAD_, _/PrepareSchema_, _/PrepareDomain_ and _/PrepareAllDomains_ commands|Specifies the domain controller that Exchange Setup uses to read from and write to Active Directory. The domain controller must meet the minimum requirements for [Exchange 2016](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2016#network-and-directory-server-requirements-for-exchange-2016) or [Exchange 2019](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019#network-and-directory-server-requirements-for-exchange-2019). <br/><br/> If you use this switch in _/PrepareSchema_ or _/PrepareAD_ commands that extend the Active Directory schema for Exchange, you must specify the schema master; otherwise, you'll get an error.|
|_/DoNotStartTransport_|n/a|n/a|`/Mode:Install /Roles:Mailbox`, `/Mode:Install /Roles:EdgeTransport`, and `/Mode:RecoverServer` commands.|Tells Setup to not start the Microsoft Exchange Transport service (mail flow) on Mailbox servers or Edge Transport servers after Setup is complete. You can use this switch to configure additional settings before the server accepts email messages (for example, configure antispam agents or move the queue database back onto a recovered Exchange server.)|
|_/EnableErrorReporting_|n/a|Disabled|`/Mode:Install`, `/Mode:Upgrade`, and `/Mode:RecoverServer` commands|Specifies whether to allow Exchange to automatically check online for solutions to errors that it encounters. You can enable or disable error reporting on the server after setup is complete by using the _ErrorReportingEnabled_ parameter on the **Set-ExchangeServer** cmdlet.|
|_/InstallWindowsComponents_|n/a|n/a|`/Mode:Install` commands|Installs the required Windows roles and features for the specified Exchange server role. If a reboot is required, Setup will resume where the installation ended.|
|_/LogFolderPath:"\<Path\>"_|A folder path (for example, "E:\Exchange Database Logs").|**%ExchangeInstallPath%Mailbox\\<DatabaseName\>** where: <br/> • \<DatabaseName> is **Mailbox Database \<10DigitNumber\>** that matches the default name of the database **or** the value you specified with the _/MdbName_ switch (without the .edb file name extension). <br/>• %ExchangeInstallPath% is **%ProgramFiles%\Microsoft\Exchange Server\V15\\** or the location you specified with the _/TargetDir_ switch.|`/Mode:Install /Roles:Mailbox` commands|Specifies the location of the transaction log files for the first mailbox database that's created on the new Mailbox server. You can specify the location of the database files with the _/DbFilePath_ switch.|
|_/MdbName:"\<FileName\>"_|A database filename without the .edb extension (for example, "db01")|**Mailbox Database \<10DigitNumber\>** (for example, **Mailbox Database 0139595516**).|`/Mode:Install /Roles:Mailbox` commands|Specifies the name of the first mailbox database that's created on the new Mailbox server. You can specify the location of the database files with the _/DbFilePath_ switch.|
|_/OrganizationName:"\<Organization Name\>"_ <br/> (or _/on:"\<Organization Name\>"_)|A text string (for example, "Contoso Corporation").|Blank in command line setup; **First Organization** in the Exchange Setup wizard.|`/Mode:Install /Roles:Mailbox` or _/PrepareAD_ commands for the first Exchange server in the organization.|The organization name is used internally by Exchange, isn't typically seen by users, doesn't affect the functionality of Exchange, and doesn't determine what you can use for email addresses. <br/>• The organization name can't contain more than 64 characters, and can't be blank. <br/>• Valid characters are A to Z, a to z, 0 to 9, hyphen or dash (-), and space, but leading or trailing spaces aren't allowed. <br/>• You can't change the organization name after it's set.|
|_/SourceDir:"\<Path\>"_ <br/> (or _/s:"\<Path\>"_)|A folder path (for example, "Z:\Exchange).|The ServerRoles\UnifiedMessaging folder on the Exchange installation media.|_/AddUmLanguagePack_ commands in Exchange 2016 (not available in Exchange 2019)|Specifies the location of the language packs (UMLanguagePack.\<Culture\>.exe files) to install on existing Exchange 2016 Mailbox servers.|
|_/TargetDir:"\<Path\>"_ <br/> (or _/t:"\<Path\>"_)|A folder path (for example, "D:\Program Files\Microsoft\Exchange").|**%ProgramFiles%\Microsoft\Exchange Server\V15\\**|`/Mode:Install` and `/Mode:RecoverServer` commands|Specifies where to install Exchange on the server. You can't install Exchange in the root of a drive (for example, C:\\), or on a ROM drive, RAM disk, network drive, removable disk, or unknown drive type. <br/> When you recover a failed Exchange server that was installed using a custom installation path, you need to use this switch to specify the custom path during the recovery.|
|_/TenantOrganizationConfig:"\<Path\>"_|A folder path (for example "C:\Data")|n/a|`/Mode:Install` or _/PrepareAD_ commands.|Required in hybrid deployments between on-premises organizations and Microsoft 365 or Office 365 to specify the location of the text file that contains the configuration information for your Microsoft 365 or Office 365 organization. You create this file by running the **Get-OrganizationConfig** cmdlet in Exchange Online PowerShell in your Microsoft 365 or Office 365 organization.|
|_/UpdatesDir:"\<Path\>"_ <br/> (or _/u:"\<Path\>"_)|A folder path (for example, "D:\Downloads\Exchange Updates").|The Updates folder at the root of the Exchange installation media.|`/Mode:Install`, `/Mode:Upgrade`, `/Mode:RecoverServer`, and _/AddUmLanguagePack_ commands.|Specifies the source location of updates for Setup to install. You can only specify one folder for updates. <br/><br/> Any UM language packs located in this folder will be **automatically** installed on the target Exchange 2016 Mailbox server.|

## What do you need to know before you begin?

- Download the latest version of Exchange on the target computer. For more information, see [Updates for Exchange Server](../../new-features/updates.md).

- Verify the network, computer hardware, operating system, and software requirements at: [Exchange Server system requirements](../../plan-and-deploy/system-requirements.md) and [Exchange Server prerequisites](../../plan-and-deploy/prerequisites.md).

- Verify that you've read the release notes at [Release notes for Exchange Server](../../release-notes.md).

  > [!CAUTION]
  > After you install Exchange on a server, you must not change the server name. Renaming a server after you've installed an Exchange server role is not supported.

- For Mailbox servers:

  - Estimated time to complete: 60 minutes

  - The target server must be a member of an Active Directory domain.

  - The account that you use to install Exchange requires the following permissions:<sup>\*</sup>:

    - **Enterprise Admins group membership**: Required if this is the first Exchange server in the organization.

    - **Schema Admins group membership**: Required if you haven't previously [extended the Active Directory schema](../../plan-and-deploy/prepare-ad-and-domains.md#step-1-extend-the-active-directory-schema) or [prepared Active Directory](../../plan-and-deploy/prepare-ad-and-domains.md#step-2-prepare-active-directory) for Exchange.

    - **Exchange Organization Management role group membership**: Required if you've already [prepared the Active Directory domain](../../plan-and-deploy/prepare-ad-and-domains.md#step-3-prepare-active-directory-domains) that will contain the Exchange server, or if other Exchange servers already exist in the organization.

    <sup>\*</sup> Members of the **Delegated Setup** role group can install Exchange on servers that have already been provisioned in Active Directory by an Exchange administrator. For more information, see [Delegate the installation of Exchange servers](../../plan-and-deploy/deploy-new-installations/delegate-installations.md).

- For Edge Transport servers:

  - Estimated time to complete: 40 minutes

  - We recommend that you install Edge Transport servers in a perimeter network that's outside of your organization's internal Active Directory forest. Installing the Edge Transport server role on domain-joined computers only enables domain management of Windows features and settings. Edge Transport servers don't directly access Active Directory. Instead, they use Active Directory Lightweight Directory Services (AD LDS) to store configuration and recipient information. For more information about the Edge Transport role, see [Edge Transport servers](../../architecture/edge-transport-servers/edge-transport-servers.md).

  - Verify the local account on the target computer is a member of the local Administrators group on the target server.

  - You need to configure the primary DNS suffix on the computer. For example, if the fully qualified domain name of your computer is edge.contoso.com, the DNS suffix for the computer is contoso.com. For more information, see [Primary DNS Suffix is missing [ms.exch.setupreadiness.FqdnMissing]](../../plan-and-deploy/deployment-ref/ms-exch-setupreadiness-fqdnmissing.md).

  - In coexistence scenarios, Exchange 2010 Hub Transport servers need an update before you can subscribe a Exchange 2016 Edge Transport server to an Active Directory site that contains Exchange 2010 Hub Transport servers. If you don't install this update, the EdgeSync Subscription won't work correctly for Exchange 2010 Hub Transport server that participate in EdgeSync synchronization. For more information, see [Supported coexistence scenarios for Exchange 2016](../system-requirements.md?view=exchserver-2016#supported-coexistence-scenarios-for-exchange-2016).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

## Use Setup.exe to install Exchange in unattended mode

1. On the target server, open File Explorer, right-click on the Exchange ISO image file that you downloaded, and then select **Mount**. Note the virtual DVD drive letter that's assigned.

2. Open a Windows Command Prompt window. For example:

   - Press the Windows key + 'R' to open the **Run** dialog, type cmd.exe, and then press **OK**.

   - Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

3. In the Command Prompt window, use the following syntax:

   ```console
   <Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms [Switches]
   ```

   Setup copies the setup files to the local computer.

   Setup checks the prerequisites, including all prerequisites specific to the server roles that you're installing. If you haven't met all the prerequisites, Setup fails and returns an error message that explains the reason for the failure. If you've met all the prerequisites, Setup installs Exchange.

4. Restart the server after the Exchange installation is complete.

5. Complete your deployment by performing the tasks provided in [Exchange Server post-installation tasks](../../plan-and-deploy/post-installation-tasks/post-installation-tasks.md).

## Unattended mode examples

### Prepare Active Directory for Exchange in unattended mode

This example configures "Fabrikam Ltd" as the Exchange organization name in Active Directory and prepares Active Directory for the version of Exchange that's being installed.

```console
Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAD /OrganizationName:"Fabrikam Ltd"
```

For more information, see [Prepare Active Directory and domains for Exchange](../prepare-ad-and-domains.md).

### Install Mailbox servers in unattended mode

- This example installs the first Exchange server (Mailbox server) in the organization, configures "Contoso Corporation" as the Exchange organization name in Active Directory, and installs the Exchange management tools on the local server.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /Mode:Install /Roles:Mailbox /on:"Contoso Corporation"
   ```

- This example installs the Mailbox server role and the management tools in the default folder on the local server in an organization where Active Directory has already been prepared for the version of Exchange that's being installed.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /mode:Install /r:MB
   ```

- This example installs the Mailbox server role and the management tools in the "C:\Exchange Server" folder on the local server.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /Mode:Install /Role:Mailbox /TargetDir:"C:\Exchange Server"
   ```

- This example installs the Mailbox server role on the local server by using the settings in the ExchangeConfig.txt file.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /mode:Install /role:Mailbox /AnswerFile:c:\ExchangeConfig.txt
   ```

- This example uses the domain controller named DC01 to read from and write to Active Directory while installing the Mailbox server role and the management tools on the local server.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /mode:Install /role:Mailbox /DomainController:DC01
   ```

- This example updates Exchange Setup with patches from the specified folder, and then installs the Mailbox server role and the management tools on the local server. In Exchange 2016 only, if any UM language packs are located in this folder, the language packs are automatically installed.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /role:Mailbox /UpdatesDir:"C:\ExchangeServer\New Patches"
   ```

### Install Edge Transport servers in unattended mode

- This example installs the Edge Transport server role and the management tools in the default location on the local server.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /mode:Install /r:EdgeTransport
   ```

- This example installs the Edge Transport server role and the management tools in the specified folder on the local server.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /mode:Install /r:ET /TargetDir:"D:\Exchange Server"
   ```

### Uninstall Exchange from servers in unattended mode

This example completely removes Exchange from the local server and removes the server's Exchange configuration from Active Directory.

```console
Setup.exe /IAcceptExchangeServerLicenseTerms /mode:Uninstall
```

### Remove provisioned Exchange server objects from Active Directory in unattended mode

This example removes the provisioned Exchange server object named Exchange03 from Active Directory _before_ Exchange is installed on the server (if Exchange is already installed on the server, the command won't work).

```console
Setup.exe /IAcceptExchangeServerLicenseTerms /rprs:Exchange03
```

For more information, see [Delegate the installation of Exchange servers](delegate-installations.md).

### Add and remove UM language packs from existing Exchange 2016 Mailbox servers in unattended mode

> [!NOTE]
> These procedures aren't available in Exchange 2019.

- This example installs the Russian and Spain Spanish language packs on the local Exchange 2016 Mailbox server from the specified folder.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /AddUmLanguagePack:ru-RU,es-ES /SourceDir:"D:\UM Language Packs"
   ```

- This example uninstalls the Korean UM language pack from the local Exchange 2016 Mailbox server.

   ```console
   Setup.exe /IAcceptExchangeServerLicenseTerms /RemoveUmLanguagePack:ko-KR
   ```

## Next steps

- To verify that you've successfully installed Exchange in unattended mode, see [Verify Exchange Server installations](../post-installation-tasks/verify-installation.md).

- Complete your deployment by performing the tasks provided in [Exchange post-installation tasks](../../plan-and-deploy/post-installation-tasks/post-installation-tasks.md).

- Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
