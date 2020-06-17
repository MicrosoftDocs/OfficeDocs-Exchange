---
title: 'Install the Exchange UM Troubleshooting Tool: Exchange 2013 Help'
TOCTitle: Install the Exchange UM Troubleshooting Tool
ms:assetid: 84223af0-a717-49ee-add6-86313bb30d17
ms:mtpsurl: https://technet.microsoft.com/library/Ff844714(v=EXCHG.150)
ms:contentKeyID: 55129211
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Install the Exchange UM Troubleshooting Tool

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

The Microsoft Exchange 2010 UM Troubleshooting Tool is an Exchange Management Shell cmdlet named **Test-ExchangeUMCallFlow**. You can use the cmdlet to diagnose configuration errors specific to call answering scenarios and to test whether voice mail is functioning correctly in both on-premises and cross-premises Microsoft Exchange Server 2010 Service Pack 1 (SP1) or later UM deployments. You can use this cmdlet in deployments with Microsoft Office Microsoft Lync Server 2010 or later or in UM deployments with Vo IP gateways, IP PBXs or session border controllers (SBCs).

The UM Troubleshooting tool can be installed on a local Unified Messaging server, an Exchange 2013 Mailbox server, or on another 64-bit computer.

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes

- The UM Troubleshooting tool requires that the following components be installed on a computer running Windows Vista, Windows 7, Windows 8, or the 64-bit edition of Windows Server 2008 or Windows Server 2012 or later before the tool is installed:

  - Microsoft .NET Framework 3.5 Service Pack 1 (SP1)   See [Microsoft .NET Framework 3.5 Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=152380).

  - If the tool will be run on a Windows Vista or Windows Server 2008 computer, see [Microsoft .NET Framework 3.5 Family Update for Windows Vista x64, and Windows Server 2008 x64](https://go.microsoft.com/fwlink/p/?linkid=178998).

  - Windows Remote Management (WinRM) 2.0 and Windows PowerShell V2 (Windows6.0-KB968930.msu). See Microsoft Knowledge Base article 968930, [Windows Management Framework Core package (Windows PowerShell 2.0 and WinRM 2.0)](https://support.microsoft.com/help/968930).

  - Microsoft Unified Communications Managed API 2.0 Core Runtime (UcmaRuntimeWebDownloadX64.msi).

  - For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Install the UM Troubleshooting Tool

1. Download the Unified Messaging Troubleshooting Tool from the [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?linkid=182625), and then double-click the MicrosoftExchange2010UMTroubleshootingTool.msi installation folder.

2. On the **Welcome to the Microsoft Exchange 2010 UM Troubleshooting Tool Setup Wizard** page, click **Next**.

3. On the **End-User License Agreement** page, review the software license terms, and if you agree, click **I accept the terms in the license agreement** and then click **Next**.

4. On the **Select Installation Folder** page, verify the path to the installation folder and click **Next**.

5. On the **Confirm Installation** page, click **Next** to start installation.

6. On the **Installation Complete** page, click **Close**.
