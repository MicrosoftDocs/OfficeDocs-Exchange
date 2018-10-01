---
title: "Exchange Server prerequisites"
ms.author: chrisda
author: chrisda
manager: serdars
ms.date: 7/19/2018
ms.audience: ITPro
ms.topic: conceptual
ms.prod: exchange-server-it-pro
localization_priority: Critical
ms.collection: Strat_EX_Admin
ms.assetid: 
description: "Summary: Learn about the Windows operating system prerequisites for Exchange Server 2016 and Exchange Server 2019 and the Exchange Management Tools."
---

# Exchange Server prerequisites

> [!TIP]
> Looking for Exchange 2013 prerequisites? See [Exchange 2013 prerequisites](https://technet.microsoft.com/library/bb691354(v=exchg.150).aspx).

This topic provides the steps for installing the necessary Windows Server operating system prerequisites for Exchange Server 2016 and Exchange Server 2019 Mailbox servers and Edge Transport servers, and also the Windows prequisites for installing the Exchange Management Tools on Windows client computers.

After you've prepared your environment for Exchange Server, use the Exchange Deployment Assistant for the next steps in your actual deployment. For information on hybrid deployments, see [Exchange Server Hybrid Deployments](https://technet.microsoft.com/library/jj200581(v=exchg.150).aspx).

> [!TIP]
> Have you heard about the Exchange Server Deployment Assistant? It is a free online tool that helps you quickly deploy Exchange Server in your organization by asking you a few questions and creating a customized deployment checklist just for you. If you want to learn more about it, go to [Microsoft Exchange Server Deployment Assistant](https://go.microsoft.com/fwlink/p/?linkid=626978).

## What do you need to know before you begin?

::: moniker range="exchserver-2019"
- Verify that your Active Directory forest functional level is Windows Server 2012 R2 or later, and that the schema master is running Windows Server 2012 R2 or later. For more information about the forest functional levels, see [Understanding Active Directory Domain Services (AD DS) Functional Levels](https://go.microsoft.com/fwlink/p/?linkId=137037).
::: moniker-end

::: moniker range="exchserver-2016"
- Verify that your Active Directory forest functional level is Windows Server 2008 R2 or higher, and that the schema master is running Windows Server 2008 R2 or later. For more information about the Windows functional level, see [Understanding Active Directory Domain Services (AD DS) Functional Levels](https://go.microsoft.com/fwlink/p/?linkId=137037).

- The full installation option of Windows Server 2012 and Windows Server 2012 R2 must be used for all servers running Exchange 2016 server roles or management tools.

- Some prerequisites require you to reboot the server to complete installation.

    > [!NOTE]
    > You can't upgrade Windows from one version to another, or from Standard to Datacenter, when Exchange is installed on the server.
::: moniker-end

- Verify the [Windows operating system requirements for Exchange Server](system-requirements.md#operating-system).

::: moniker range="exchserver-2019"
    > [!NOTE]
    > New to Exchange 2019 is the ability to upgrade your operating system to a newer version while Exchange is installed on Windows Server 2019 or later.
::: moniker-end
    
- Verify the computer is joined to the appropriate internal Active Directory domain.

- Install the latest Windows updates on your computer. For more information, see [Deployment Security Checklist](https://technet.microsoft.com/library/aa996026(v=exchg.150).aspx).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).


::: moniker range="exchserver-2019"
## Exchange 2019 prerequisites for preparing Active Directory

You can use any member of the Active Directory domain to prepare Active Directory for Exchange 2019.

1. The computer requires the following software:

    a. [.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265) or later

    b. [Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679)

    **Note**: The Visual C++ Redistributable package is required if you're using the Exchange Setup Wizard to prepare Active Directory. If you're using unattended Setup from the command line to prepare Active Directory, this package isn't required. For more information, see [Prepare Active Directory and domains](prepare-ad-and-domains.md).
    
2. Install the Remote Tools Administration Pack by running the following command in Windows PowerShell:

    ```
    Install-WindowsFeature RSAT-ADDS
    ```

    **Note**: Using the Exchange Setup Wizard to prepare Active Directory requires the installation of the Management Tools Exchange role.

## Windows Server 2019 prerequisites for Exchange 2019

The requirements to install Exchange 2019 on Windows Server 2019 computers are described in the following sections. We recommend either of the following methods to install the Windows prerequisites for Exchange 2019:

- Use the /InstallWindowsComponents switch in unattended Setup mode.

- Select the check box in the Exchange Setup Wizard to install Windows prerequisites.
 
When you use one of these options, you don't need to restart the computer after the Windows components have been added.

### Exchange 2019 Mailbox servers on Windows Server 2019

1. Install the following software:

    a. [.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265) or later

    b. [Visual C++ Redistributable Package for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=2002913).

2. Add the required Lync Server or Skype for Business Server components:

    a. Install the Server Media Foundation windows feature by executing the following command in Windows PowerShell:
    
      ```
      Install-WindowsFeature Server-Media-Foundation`
      ```
    
   b. Install [Unified Communications Managed API 4.0](https://www.microsoft.com/download/details.aspx?id=34992). This package is available for download and in the \UCMARedist folder on the Exchange Server media.

    > [!NOTE]
    > When installing on Windows Server Core, you must use the installation package located in \UCMARedist on distributed media.

3. If you aren't going to use Exchange Setup to install the required Windows components (in the wizard or from the command line), run the one of the following commands in Windows PowerShell:

   - **Desktop Experience**:

      ```
      Install-WindowsFeature AS-HTTP-Activation, Server-Media-Foundation, NET-Framework-45-Features, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, Web-Mgmt-Console, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
      ```

   - **Server Core**:

      ```
      Install-WindowsFeature AS-HTTP-Activation, Server-Media-Foundation, NET-Framework-45-Features, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Metabase, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
      ```
    
### Exchange 2019 Edge Transport servers on Windows Server 2019

1. Install the [Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679).

2. If you aren't going to use Exchange Setup to install the required Windows components (in the wizard or from the command line), run the following command in Windows PowerShell:

    ```
    Install-WindowsFeature ADLDS
    ```

## Windows 10 client prerequisites for the Exchange 2019 management tools

1. Install the [Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679).

2. If you aren't going to use Exchange Setup to install the required Windows components (in the wizard or from the command line), run the following command in Windows PowerShell:

    ```
    Enable-WindowsOptionalFeature -Online -FeatureName IIS-ManagementScriptingTools,IIS-ManagementScriptingTools,IIS-IIS6ManagementCompatibility,IIS-LegacySnapIn,IIS-ManagementConsole,IIS-Metabase,IIS-WebServerManagementTools,IIS-WebServerRole
    ```
::: moniker-end

::: moniker range="exchserver-2016"
## Exchange 2016 prerequisites for preparing Active Directory

You can use any member of the Active Directory domain to prepare Active Directory for Exchange 2016.

1. The computer requires the following software:

    a. [.NET Framework 4.7.1](https://go.microsoft.com/fwlink/p/?linkid=866906)

    b. [Visual C++ Redistributable Packages for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=2002913)

2. Install the Remote Tools Administration Pack by running the following command in Windows PowerShell:

    ```
    Install-WindowsFeature RSAT-ADDS
    ```

After you've installed the Remote Tools Administration Pack you can use the computer to prepare Active Directory. For more information, see [Prepare Active Directory and domains](prepare-ad-and-domains.md).

## Windows Server 2016 prerequisites for Exchange 2016

The prerequisites that are needed to install Exchange 2016 on computers running Windows Server 2016 depends on which Exchange role you want to install. Read the section below that matches the role you want to install.

  > [!IMPORTANT]
  > Windows Server 2016 requires Exchange 2016 Cumulative Update 3 or later.

### Exchange 2016 Mailbox servers on Windows Server 2016

1. Run the following command in Windows PowerShell to install the required Windows components:

    ```
    Install-WindowsFeature NET-Framework-45-Features, Server-Media-Foundation, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, Web-Mgmt-Console, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
    ```

2. Install the following software in order:

    a. [.NET Framework 4.7.1](https://go.microsoft.com/fwlink/p/?linkid=866906)

    b. [Microsoft Knowledge Base article KB3206632](https://go.microsoft.com/fwlink/p/?linkid=837748)

    c. [Visual C++ Redistributable Packages for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=2002913)

    d. [Microsoft Unified Communications Managed API 4.0, Core Runtime 64-bit](https://go.microsoft.com/fwlink/p/?linkId=258269)

### Exchange 2016 Edge Transport servers on Windows Server 2016

1. Run the following command in Windows PowerShell to install the required Windows components:

    ```
    Install-WindowsFeature ADLDS
    ```

2. Install the following software in order:

    a. [.NET Framework 4.7.1](https://go.microsoft.com/fwlink/p/?linkid=866906)

    b. [Visual C++ Redistributable Packages for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=2002913)

## Windows Server 2012 and Windows Server 2012 R2 prerequisites for Exchange 2016

The prerequisites for Exchange 2016 on Windows Server 2012 or Windows Server 2012 R2 computers depend on the Exchange role that you're installing. Read the following section that matches the role you want to install.

### Exchange 2016 Mailbox servers on Windows Server 2012 or Windows Server 2012 R2

1. Run the following command in Windows Powershell to install the required Windows components:

  ```
  Install-WindowsFeature AS-HTTP-Activation, Server-Media-Foundation, NET-Framework-45-Features, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, Web-Mgmt-Console, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
  ```

2. Install the following software in order:

    a. [.NET Framework 4.7.1](https://go.microsoft.com/fwlink/p/?linkid=866906)

    b. [Visual C++ Redistributable Packages for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=2002913)

    c. [Microsoft Unified Communications Managed API 4.0, Core Runtime 64-bit](https://go.microsoft.com/fwlink/p/?linkId=258269)

### Exchange 2016 Edge Transport servers on Windows Server 2012 or Windows Server 2012 R2

1. Run the following command in Windows PowerShell to install the required Windows components:

    ```
    Install-WindowsFeature ADLDS
    ```

2. Install the following software in order:

    a. [.NET Framework 4.7.1](https://go.microsoft.com/fwlink/p/?linkid=866906)

    b. [Visual C++ Redistributable Packages for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=2002913)

## Windows client prerequisites for the Exchange 2016 management tools

### Exchange 2016 management tools on Windows 10

1. Install [Visual C++ Redistributable Packages for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=2002913).

2. Run the following command in an elevated Windows PowerShell window (a Windows PowerShell window you open by selecting **Run as administrator**): 

    ```
    Enable-WindowsOptionalFeature -Online -FeatureName IIS-ManagementScriptingTools,IIS-ManagementScriptingTools,IIS-IIS6ManagementCompatibility,IIS-LegacySnapIn,IIS-ManagementConsole,IIS-Metabase,IIS-WebServerManagementTools,IIS-WebServerRole
    ```

### Exchange 2016 management tools on Windows 8.1

1. Install [.NET Framework 4.7.1](https://go.microsoft.com/fwlink/p/?linkid=866906).

2. Install [Visual C++ Redistributable Packages for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=2002913).

3. Run the following command in an elevated Windows PowerShell window (a Windows PowerShell window you open by selecting **Run as administrator**): 

    ```
    Enable-WindowsOptionalFeature -Online -FeatureName IIS-ManagementScriptingTools,IIS-ManagementScriptingTools,IIS-IIS6ManagementCompatibility,IIS-LegacySnapIn,IIS-ManagementConsole,IIS-Metabase,IIS-WebServerManagementTools,IIS-WebServerRole
    ```
::: moniker-end
