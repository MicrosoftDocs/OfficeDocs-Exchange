---
title: 'Exchange 2013 prerequisites: Exchange 2013 Help'
TOCTitle: Exchange 2013 prerequisites
ms:assetid: e21cf744-7813-48b3-9293-5cecd89a6c25
ms:mtpsurl: https://technet.microsoft.com/library/Bb691354(v=EXCHG.150)
ms:contentKeyID: 48385640
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013 prerequisites

_**Applies to:** Exchange Server 2013_

This topic provides the steps for installing the necessary Windows Server 2012 R2, Windows Server 2012 and Windows Server 2008 R2 with Service Pack 1 (SP1) operating system prerequisites for the Microsoft Exchange 2013 Mailbox, Client Access, and Edge Transport server roles. It also provides the prerequisites required to install the Exchange 2013 management tools on Windows 8, Windows 8.1, and Windows 7 client computers.

## What do you need to know before you begin?

- The information in this topic is applicable to Service Pack 1 and later versions of Exchange 2013.

- The Edge Transport server role is available starting with Exchange 2013 SP1.

- Make sure that the functional level of your forest is at least Windows Server 2003, and that the schema master is running Windows Server 2003 with Service Pack 2 or later. For more information about the Windows functional level, see [Managing Domains and Forests](https://go.microsoft.com/fwlink/p/?linkid=137037).

- The full installation option of Windows Server 2012 R2, Windows Server 2012 and Windows Server 2008 R2 SP1 must be used for all servers running Exchange 2013 server roles or management tools.

- You must first join the computer to the appropriate internal Active Directory forest and domain.

- Some prerequisites require you to reboot the server to complete installation.

  - Install the latest Windows updates on your computer. For more information, see [Deployment security checklist](deployment-security-checklist-exchange-2013-help.md).

    > [!NOTE]
    > If you're installing the Mailbox server role and you intend for the server to be a member of a database availability group (DAG), you must be running Windows Server 2012 R2 Standard or Datacenter Edition, Windows Server 2012 Standard or Datacenter Edition, or Windows Server 2008 R2 SP1 Enterprise Edition. Windows Server 2008 R2 SP 1 Standard Edition doesn't support the features needed for DAGs.<BR>You can't upgrade Windows when Exchange is installed on the server.<BR>To upgrade to Microsoft Unified Communications Managed API (UCMA) 4.0, you must first uninstall any previous versions of UCMA that are installed by using <STRONG>Add/Remove programs</STRONG>.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Active Directory preparation

The computer you want to use to prepare Active Directory for Exchange 2013 has specific prerequisites that must be met.

Install the following software, in the order shown, on the computer that will be used to prepare Active Directory:

1. [.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265)

2. [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/p/?linkid=390234) (included with Windows Server 2012 R2)

After you've installed the software listed above, complete the following steps to install the Remote Tools Administration Pack. After you've installed the Remote Tools Administration Pack you'll be able to use the computer to prepare Active Directory. For more information about preparing Active Directory, see [Prepare Active Directory and domains](prepare-active-directory-and-domains-exchange-2013-help.md).

1. Open Windows PowerShell.

2. Install the Remote Tools Administration Pack.

   - On a Windows Server 2012 R2 or Windows Server 2012 computer, run the following command.

     ```powershell
     Install-WindowsFeature RSAT-ADDS
     ```

   - On a Windows Server 2008 R2 SP1 computer, run the following command.

     ```powershell
     Add-WindowsFeature RSAT-ADDS
     ```

## Windows Server 2012 R2 and Windows Server 2012 prerequisites

The prerequisites that are needed to install Exchange 2013 on a Windows Server 2012 R2 or Windows Server 2012 computer depends on which Exchange roles you want to install. Read the section below that matches the roles you want to install.

## Mailbox or Client Access server roles

Follow the instructions in this section to install the prerequisites on Windows Server 2012 R2 or Windows Server 2012 computers where you want to do one of the following:

- Install only the Mailbox server role on a computer.

- Install only the Client Access server role on a computer.

- Install both the Mailbox and Client Access server roles on the same computer.

Do the following to install the required Windows roles and features:

1. Open Windows PowerShell.

2. Run the following command to install the required Windows components.

    ```powershell
    Install-WindowsFeature AS-HTTP-Activation, Desktop-Experience, NET-Framework-45-Features, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, Web-Mgmt-Console, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
    ```

After you've installed the operating system roles and features, install the following software in the order shown:

1. [.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265)

    > [!IMPORTANT]
    > Exchange 2013 CU23 **requires** .NET Framework 4.7.2. Upgrade your servers to .NET Framework 4.7.2 **before** you install Exchange 2013 CU23 or you'll receive an error. If you're running .NET Framework 4.7.1, upgrade to Exchange 2013 CU22 before you install .NET Framework 4.7.2. If you're running .NET Framework 4.6.2, upgrade to Exchange 2013 CU20 before you install .NET Framework 4.7.1.

2. [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/p/?linkid=390234) (included with Windows Server 2012 R2)

3. [Microsoft Unified Communications Managed API 4.0, Core Runtime 64-bit](https://go.microsoft.com/fwlink/p/?linkid=258269)

4. [Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679)

5. [Visual C++ Redistributable Package for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)

  > [!NOTE]
  > Here you'll find an overview of the latest supported [Visual C++ Redistributable versions](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## Edge Transport server role

Follow the instructions in this section to install the prerequisites on Windows Server 2012 R2 or Windows Server 2012 computers where you want to install the Edge Transport server role on a computer.

Do the following to install the required Windows roles and features:

1. Open Windows PowerShell.

2. Run the following command to install the required Windows components.

    ```powershell
    Install-WindowsFeature ADLDS
    ```

Install the version of Microsoft .NET Framework that corresponds to the version of Exchange 2013 you're installing:

1. [.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265)

    > [!IMPORTANT]
    > Exchange 2013 CU23 **requires** .NET Framework 4.7.2. Upgrade your servers to .NET Framework 4.7.2 **before** you install Exchange 2013 CU23 or you'll receive an error. If you're running .NET Framework 4.7.1, upgrade to Exchange 2013 CU22 before you install .NET Framework 4.7.2. If you're running .NET Framework 4.6.2, upgrade to Exchange 2013 CU20 before you install .NET Framework 4.7.1.

2. [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/p/?linkid=390234) (included with Windows Server 2012 R2)

3. [Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679)

  > [!NOTE]
  > Here you'll find an overview of the latest supported [Visual C++ Redistributable versions](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## Windows Server 2008 R2 SP1 prerequisites

The prerequisites that are needed to install Exchange 2013 on a Windows Server 2008 R2 SP1 computer depends on which Exchange roles you want to install. Read the section below that matches the roles you want to install.

## Mailbox or Client Access server roles

Follow the instructions in this section to install the prerequisites on Windows Server 2008 R2 SP1 computers where you want to do one of the following:

- Install only the Mailbox server role on a computer.

- Install only the Client Access server role on a computer.

- Install both the Mailbox and Client Access server roles on the same computer.

Do the following to install the required Windows roles and features:

1. Open Windows PowerShell.

2. Run the following command to load the Server Manager module.

    ```powershell
    Import-Module ServerManager
    ```

3. Run the following command to install the required Windows components.

   ```powershell
   Add-WindowsFeature Desktop-Experience, NET-Framework, NET-HTTP-Activation, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Web-Server, WAS-Process-Model, Web-Asp-Net, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, RSAT-ADDS
   ```

After you've installed the operating system roles and features, install the following software in the order shown:

1. [.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265)

    > [!IMPORTANT]
    > Exchange 2013 CU23 **requires** .NET Framework 4.7.2. Upgrade your servers to .NET Framework 4.7.2 **before** you install Exchange 2013 CU23 or you'll receive an error. If you're running .NET Framework 4.7.1, upgrade to Exchange 2013 CU22 before you install .NET Framework 4.7.2. If you're running .NET Framework 4.6.2, upgrade to Exchange 2013 CU20 before you install .NET Framework 4.7.1.

2. [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/p/?linkid=390234)

3. [Microsoft Unified Communications Managed API 4.0, Core Runtime 64-bit](https://go.microsoft.com/fwlink/p/?linkid=258269)

4. [Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679)

5. [Visual C++ Redistributable Package for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)

   > [!NOTE]
   > Here you'll find an overview of the latest supported [Visual C++ Redistributable versions](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

6. [Microsoft Knowledge Base article KB974405 (Windows Identity Foundation)](https://support.microsoft.com/help/974405)

7. [Knowledge Base article KB2619234 (Enable the Association Cookie/GUID that is used by RPC over HTTP to also be used at the RPC layer in Windows 7 and in Windows Server 2008 R2)](https://support.microsoft.com/help/2619234)

8. [Knowledge Base article KB2533623 (Insecure library loading could allow remote code execution)](https://support.microsoft.com/help/2533623)

    > [!NOTE]
    > This hotfix may already be installed if you've configured Windows Update to install security updates on your computer.

## Edge Transport server role

Follow the instructions in this section to install the prerequisites on Windows Server 2008 R2 SP1 computers where you want to install the Edge Transport server role on a computer.

Do the following to install the required Windows roles and features:

1. Open Windows PowerShell.

2. Run the following command to load the Server Manager module.

    ```powershell
    Import-Module ServerManager
    ```

3. Run the following command to install the required Windows components.

    ```powershell
    Add-WindowsFeature NET-Framework, ADLDS
    ```

After you've installed the operating system roles and features, install the following software in the order shown:

1. [.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265)

   > [!IMPORTANT]
   > Exchange 2013 CU23 **requires** .NET Framework 4.7.2. Upgrade your servers to .NET Framework 4.7.2 **before** you install Exchange 2013 CU23 or you'll receive an error. If you're running .NET Framework 4.7.1, upgrade to Exchange 2013 CU22 before you install .NET Framework 4.7.2. If you're running .NET Framework 4.6.2, upgrade to Exchange 2013 CU20 before you install .NET Framework 4.7.1.

2. [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/p/?linkid=390234)

3. [Microsoft Unified Communications Managed API 4.0, Core Runtime 64-bit](https://go.microsoft.com/fwlink/p/?linkid=258269)

4. [Visual C++ Redistributable Package for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30679)

   > [!NOTE]
   > Here you'll find an overview of the latest supported [Visual C++ Redistributable versions](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## Windows 7 prerequisites (admin tools only)

Follow the instructions in this section to install the prerequisites on domain-joined Windows 7 64-bit computers where you want to install the Exchange management tools.

1. Open **Control Panel**, and then select **Programs**.

2. Click **Turn Windows features on or off**.

3. Navigate to **Internet Information Services** \> **Web Management Tools** \> **IIS 6 Management Compatibility**.

4. Select the check box for **IIS 6 Management Console**, and then click **OK**.

After you've installed the operating system features, install the following software in the order shown:

1. [.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265)

2. [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/p/?linkid=390234)

3. [Knowledge Base article KB974405 (Windows Identity Foundation)](https://support.microsoft.com/help/974405)

## Windows 8 and Windows 8.1 prerequisites (admin tools only)

[.NET Framework 4.7.2](https://go.microsoft.com/fwlink/p/?linkid=863265)
