---
title: 'Verify an Exchange 2013 installation: Exchange 2013 Help'
TOCTitle: Verify an Exchange 2013 installation
ms:assetid: fdd20a2a-c8c1-4d17-b813-3c05d88a4411
ms:mtpsurl: https://technet.microsoft.com/library/Bb125254(v=EXCHG.150)
ms:contentKeyID: 48289471
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Verify an Exchange 2013 installation

_**Applies to:** Exchange Server 2013_

After you install Microsoft Exchange Server 2013, we recommend that you verify the installation by running the **Get-ExchangeServer** cmdlet and by reviewing the setup log file. If the setup process fails or errors occur during installation, you can use the setup log file to track down the source of the problem.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.

## Run Get-ExchangeServer

To verify that Exchange 2013 installed successfully, run the **Get-ExchangeServer** cmdlet in the Exchange Management Shell. A list is displayed of all Exchange 2013 server roles that are installed on the specified server when this cmdlet is run.

For detailed syntax and parameter information, see [Get-ExchangeServer](https://docs.microsoft.com/powershell/module/exchange/Get-ExchangeServer).

## Review the setup log file

You can also learn more about the installation and configuration of Exchange 2013 by reviewing the setup log file created during the setup process.

During installation, Exchange Setup logs events in the **Application** log of **Event Viewer** on computers that are running Windows Server 2008 R2 with Service Pack 1 (SP1) and Windows Server 2012. Review the **Application** log, and make sure there are no warning or error messages related to Exchange setup. These log files contain a history of each action that the system takes during Exchange 2013 setup and any errors that may have occurred. By default, the logging method is set to `Verbose`. Information is available for each installed server role.

You can find the setup log file at *\<system drive\>*\\ExchangeSetupLogs\\ExchangeSetup.log. The *\<system drive\>* variable represents the root directory of the drive where the operating system is installed.

The setup log file tracks the progress of every task that is performed during the Exchange 2013 installation and configuration. The file contains information about the status of the prerequisite and system readiness checks that are performed before installation starts, the application installation progress, and the configuration changes that are made to the system. Check this log file to verify that the server roles were installed as expected.

We recommend that you start your review of the setup log file by searching for any errors. If you find an entry that indicates that an error occurred, read the associated text to determine the cause of the error.
