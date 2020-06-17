---
title: 'Test and troubleshoot with the UM Troubleshooting Tool: Exchange 2013 Help'
TOCTitle: Testing and troubleshooting with the UM Troubleshooting Tool
ms:assetid: 1fab2e52-bd2d-4e46-b222-53fee9d34cba
ms:mtpsurl: https://technet.microsoft.com/library/Gg621148(v=EXCHG.150)
ms:contentKeyID: 55129208
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Testing and troubleshooting with the UM Troubleshooting Tool in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

The Microsoft Exchange 2010 UM Troubleshooting Tool is an Exchange Management Shell cmdlet named **Test-ExchangeUMCallFlow**. You can use the cmdlet to diagnose configuration errors specific to call answering scenarios and to test whether voice mail is functioning correctly in both on-premises and cross-premises Microsoft Exchange Server 2010 Service Pack 1 (SP1) or later UM deployments. You can use this cmdlet in deployments with Microsoft Office Microsoft Lync Server 2010 or later or in UM deployments with Vo IP gateways, IP PBXs or session border controllers (SBCs).

## Overview

This cmdlet emulates calls and runs a series of diagnostic tests that help on-premises administrators to identify configuration errors in telephony equipment, Exchange 2010 SP1 or later Unified Messaging settings, and connectivity issues between on-premises and cross-premises deployments of Exchange 2010 SP1 or later Unified Messaging.

When you run the cmdlet, it states the reason and possible solutions for issues that have been detected. It also outputs general audio quality metrics for diagnosing audio quality issues related to network connectivity, such as jitter and average packet loss. The **Test-ExchangeUMCallFlow** cmdlet supports testing UM components in `Secured`, `SIP Secured`, and `Unsecured` calls, and it can be run either in `Gateway` or `SIPClient` modes.

By default, when you're running the UM Troubleshooting Tool, it uses the credentials that are used when you log on to the computer. The credentials used are those that are specified for the calling party. You must set or specify the credentials to be used when you're running the UM Troubleshooting Tool in `SIPClient` mode. However, you don't need to set the credentials when running the UM Troubleshooting Tool in `Gateway` mode. If you will be using the UM Troubleshooting Tool in `SIPClient` mode, several other Office Communications Server 2007 R2 or Lync Server requirements and prerequisites must be met. For more information, see [Checklist: Deploy Office Communications Server 2007 R2 and Exchange 2010 Unified Messaging](https://go.microsoft.com/fwlink/p/?linkid=311961) or [Checklist: Integrate Exchange 2013 UM with Lync Server](checklist-integrate-exchange-2013-um-with-lync-server-exchange-2013-help.md).

> [!IMPORTANT]
> The <STRONG>Test-ExchangeUMCallFlow</STRONG> cmdlet must be used to test only the voice mail functionality of a Microsoft Exchange Server 2010 Unified Messaging server that has Exchange 2010 Service Pack 1 (SP1) installed or Microsoft Exchange 2013.

The **Test-ExchangeUMCallFlow** cmdlet can be installed on a local Exchange 2010 Unified Messaging server, an Exchange 2013 Mailbox server, or on another 64-bit computer running:

- The Windows 7 or Windows 8 operating system

- The Windows Server 2008 or Windows Server 2008 R2 operating system

- The Windows Server 2012 or Windows Server 2012 R2 operating system

The **Test-ExchangeUMCallFlow** cmdlet requires the following components to be installed on a Windows 7, Windows 8, Windows Server 2008, or Windows Server 2012 64-bit computer before installing the cmdlet:

- Microsoft .NET Framework 3.5 Service Pack 1 (SP1). To download the service pack, see [Microsoft .NET Framework 3.5 Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=152380).

- Microsoft .NET Framework 3.5 Family Update for Windows Vista x64 and Windows Server 2008 x64 updates if the tool will be run on a Windows Vista or Windows Server 2008 computer. To download the update, see [Microsoft .NET Framework 3.5 Family Update for Windows Vista x64, and Windows Server 2008 x64](https://go.microsoft.com/fwlink/p/?linkid=178998).

- Windows Remote Management (WinRM) 2.0 and Windows PowerShell V2 (Windows6.0-KB968930.msu). For more information, see Microsoft Knowledge Base article 968930, [Windows Management Framework core package (Windows PowerShell 2.0 and WinRM 2.0)](https://support.microsoft.com/help/968930).

- Unified Communications Managed AP1 2.0, Core Runtime (64-bit).

The **Test-ExchangeUMCallFlow** cmdlet isn't included on the Exchange 2010 SP1 DVD, the Exchange 2010 SP1-only download, or the Exchange 2013 installation media; however, you can download the cmdlet from the [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?linkid=182625).

For more information about syntax and parameters, see [Test-ExchangeUMCallFlow](https://www.microsoft.com/download/details.aspx?id=20839).
