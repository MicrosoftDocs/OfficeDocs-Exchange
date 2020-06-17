---
title: 'Install the Exchange 2013 management tools: Exchange 2013 Help'
TOCTitle: Install the Exchange 2013 management tools
ms:assetid: 71fcbe4c-783b-4f77-aabb-a21aa7a4ef23
ms:mtpsurl: https://technet.microsoft.com/library/Bb232090(v=EXCHG.150)
ms:contentKeyID: 49289303
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Install the Exchange 2013 management tools

_**Applies to:** Exchange Server 2013_

With the Microsoft Exchange Server 2013 management tools, you can configure and manage your Exchange organization remotely. Exchange 2013 management tools include the Exchange Management Shell and the Exchange Toolbox. This topic explains how you can either use Setup.exe or unattended setup mode to install the Exchange 2013 management tools.

> [!NOTE]
> You don't need to perform this procedure to use the Exchange admin center (EAC) remotely. The EAC is a web-based console that's hosted on computers running the Exchange 2013 Client Access server role. For more information about accessing the EAC remotely, see <A href="exchange-admin-center-in-exchange-2013-exchange-2013-help.md">Exchange admin center in Exchange 2013</A>.

For more information about managing Exchange 2013, see [Exchange admin center in Exchange 2013](exchange-admin-center-in-exchange-2013-exchange-2013-help.md) and [Using PowerShell with Exchange 2013 (Exchange Management Shell)](https://docs.microsoft.com/powershell/exchange/exchange-management-shell).

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes

- Make sure you've read the release notes prior to installing Exchange 2013. For more information, see [Release notes for Exchange 2013](release-notes-for-exchange-2013-exchange-2013-help.md).

- The computer you install the management tools on must have a supported operating system (such as Windows Server 2012 or Windows 8), have enough disk space, be a member of an Active Directory domain, and satisfy other requirements. For information about system requirements, see [Exchange 2013 system requirements](exchange-2013-system-requirements-exchange-2013-help.md).

- To run Exchange 2013 Setup, you must install Microsoft .NET Framework 4.5, Windows Management Framework 3.0, and other required software. To understand the prerequisites for all server roles, see [Exchange 2013 prerequisites](exchange-2013-prerequisites-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use Setup to install the Exchange 2013 management tools

1. Log on to the computer on which you want to install Exchange 2013.

2. Navigate to the network location of the Exchange 2013 installation files.

3. Start Exchange 2013 Setup by double-clicking `Setup.exe`

    > [!IMPORTANT]
    > If you have User Access Control (UAC) enabled, you must right-click <CODE>Setup.exe</CODE> and select <STRONG>Run as administrator</STRONG>.

4. On the **Check for Updates** page, choose whether you want Setup to connect to the Internet and download product and security updates for Exchange 2013. If you select **Connect to the Internet and check for updates**, Setup will download updates and apply them prior to continuing. If you select **Don't check for updates right now**, you can download and install updates manually later. We recommend that you download and install updates now. Click **Next** to continue.

5. The **Introduction** page begins the process of installing Exchange into your organization. It will guide you through the installation. Several links to helpful deployment content are listed. We recommend that you visit these links prior to continuing setup. Click **Next** to continue.

6. On the **License Agreement** page, review the software license terms. If you agree to the terms, select **I accept the terms in the license agreement**, and then click **Next**.

7. On the **Recommended settings** page, select whether you want to use the recommended settings. If you select **Use recommended settings**, Exchange will automatically send error reports and information about your computer hardware and how you use Exchange to Microsoft. If you select **Don't use recommended settings**, these settings remain disabled but you can enable them at any time after Setup completes. For more information about these settings and how information sent to Microsoft is used, click **?**.

8. On the **Server Role Selection** page, verify that **Management Tools** is selected.

    Select **Automatically install Windows Server roles and features that are required to install Exchange Server** to have the Setup wizard install required Windows prerequisites. You may need to reboot the computer to complete the installation of some Windows features. If you don't select this option, you must install the Windows features manually.

    > [!NOTE]
    > This option installs only the Windows features required by Exchange. You must manually install other prerequisites manually. For more information, see <A href="exchange-2013-prerequisites-exchange-2013-help.md">Exchange 2013 prerequisites</A>.

    Click **Next** to continue.

9. On the **Installation Space and Location** page, either accept the default installation location or click **Browse** to choose a new location. Make sure that you have enough disk space available in the location where you want to install Exchange. Click **Next** to continue.

10. If this is the first time you've run Exchange 2013 Setup in your organization, on the **Exchange Organization** page, type a name for your Exchange organization. The Exchange organization name can contain only the following characters:

    - A through Z

    - a through z

    - 0 through 9

    - Space (not leading or trailing)

    - Hyphen or dash

    > [!NOTE]
    > The organization name can't contain more than 64 characters. The organization name can't be blank.

    If you want to use the Active Directory split permissions model, select **Apply Active Directory split permission security model to the Exchange organization**.

    > [!WARNING]
    > Most organizations don't need to apply the Active Directory split permissions model. If you need to separate management of Active Directory security principals and Exchange configuration, Role Based Access Control (RBAC) split permissions might work for you. For more information, click <STRONG>?</STRONG>.

    Click **Next** to continue.

11. On the **Readiness Checks** page, view the status to determine if the organization and server role prerequisite checks completed successfully. If they haven't completed successfully, you must resolve any reported errors before you can install Exchange 2013. You don't need to exit Setup when resolving some of the prerequisite errors. After resolving a reported error, click **back** and then click **Next** to run the prerequisite check again. Be sure to also review any warnings that are reported. If all readiness checks have completed successfully, click **Next** to install Exchange 2013.

12. On the **Completion** page, click **Finish**.

13. Restart the computer after Exchange 2013 has completed.

## Use unattended Setup mode to install the Exchange 2013 management tools

1. Log on to the computer on which you want to install the Exchange 2013 management tools.

2. Navigate to the network location of the Exchange 2013 installation files.

3. At the command prompt, run the following command.

   > [!IMPORTANT]
   > If you have User Access Control (UAC) enabled, you must run <CODE>Setup.exe</CODE> from an elevated command prompt.

   ```powershell
   Setup.exe /Role:ManagementTools /IAcceptExchangeServerLicenseTerms
   ```

For more information, see [Install Exchange 2013 using unattended mode](install-exchange-2013-using-unattended-mode-exchange-2013-help.md).
