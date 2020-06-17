---
localization_priority: Normal
description: 'Summary: Learn how to verify ord troubleshoot your Exchange 2016 or Exchange 2019 installation.'
ms.topic: get-started-article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: fdd20a2a-c8c1-4d17-b813-3c05d88a4411
ms.reviewer:
title: Verify Exchange Server installations
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Verify Exchange Server installations

After you install Exchange Server 2016 or Exchange Server 2019, we recommend that you verify the installation by running the **Get-ExchangeServer** cmdlet and by reviewing the Exchange Setup log. If the setup process fails or errors occur during installation, you can use the Setup log to find the source of the problem.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Run Get-ExchangeServer

To verify that Exchange installed successfully, run the following commands in the Exchange Management Shell. To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

This command returns a summary list of the names, Active Directory sites, Exchange server roles, Exchange editions, and Exchange versions of all Exchange servers in the organization.

```powershell
Get-ExchangeServer
```

This example returns additional details about the Exchange server named Mailbox01.

```powershell
Get-ExchangeServer -Identity Mailbox01 | Format-List
```

For detailed syntax and parameter information, see [Get-ExchangeServer](https://docs.microsoft.com/powershell/module/exchange/get-exchangeserver).

## Review the Windows Application log and the Exchange Setup log

- Exchange Setup logs events in the **Application** log of the Windows Server. This log contains a history of each action that the system takes during Exchange setup and any errors that occurred (By default, the logging method is set to Verbose). You can use the Windows **Event Viewer** to find the  messages related to Exchange setup.

- The Exchange Setup log is available at _\<system drive\>_:\ExchangeSetupLogs\ExchangeSetup.log (_\<system drive\>_ is the drive where Windows is installed). The Setup log tracks the progress of every task during the Exchange installation and configuration. The file contains information about the status of the prerequisite and system readiness checks before installation starts, the application installation progress, and the configuration changes that are made to the system. Check this log file to verify that Exchange was installed as expected.

We recommend that you start your review of the Windows Application log and/or the Exchange Setup log by searching for errors. If you find an error entry, read the associated text to determine the cause of the error.
