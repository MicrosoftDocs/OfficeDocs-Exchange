---
title: 'Configure Edge Transport server using cloned configuration: Exchange 2013 Help'
TOCTitle: Configure Edge Transport server using cloned configuration
ms:assetid: 0bbc83e3-e5e8-4480-a8a6-15f035360856
ms:mtpsurl: https://technet.microsoft.com/library/Aa996008(v=EXCHG.150)
ms:contentKeyID: 61200276
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Edge Transport server using cloned configuration

_**Applies to:** Exchange Server 2013_

You can use the provided Exchange Management Shell scripts (located in %ExchangeInstallPath%Scripts) to duplicate the configuration of an Edge Transport server. This process is referred to as *cloned configuration*. *Cloned configuration* is the practice of deploying new Edge Transport servers based on configuration information from a previously configured source server. The configuration information from the previously configured source server is copied and exported to an XML file, which is then imported to the target server. For an overview of this process, see [Edge Transport server cloned configuration](edge-transport-server-cloned-configuration-exchange-2013-help.md).

Edge Transport server configuration information is stored in Active Directory Lightweight Directory Services (AD LDS) and isn't replicated among multiple Edge Transport servers. Using cloned configuration, you can ensure that every Edge Transport server deployed in your perimeter network is using the same configuration.

Two Shell scripts are used to perform cloned configuration tasks:

  - **ExportEdgeConfig.ps1**: Exports all user-configured settings and data from an Edge Transport server and stores that data in an XML file.

  - **ImportEdgeConfig.ps1**: During the validate configuration step, the ImportEdgeConfig.ps1 script checks the exported XML file to see whether the server-specific export settings are valid for the target server. If settings need to be modified, the script writes the invalid settings to an answer file you can modify to specify target server information, which is then used during the import configuration step. During the import configuration step, the script imports all user-configured settings and data stored in the intermediate XML file created by the ExportEdgeConfig.ps1 script.

Both these scripts are located in the %ExchangeInstallPath%Scripts folder.

## Before you begin

- Estimated time to complete this task: 30 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Edge Transport servers" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- Cloned configuration doesn't duplicate a server's Edge Subscription settings. EdgeSync certificates aren't cloned. You need to run the EdgeSync process separately for each Edge Transport server. EdgeSync overwrites any settings included in both cloned configuration information and in EdgeSync replication information.

- If any Send connectors are configured to use credentials, the password is written to the intermediate XML file as an encrypted string. You can use the *-key* parameter with the ImportEdgeConfig.ps1 and ExportEdgeConfig.ps1 scripts to specify the 32-byte string to use for password encryption and decryption. If you don't use the *-key* parameter, a default encryption key is used.

- You can only use the Shell to perform this procedure. To learn how to open the Shell in your on-premises Exchange organization, see [Open the Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Export the source server configuration data to a file on the source server

1. Copy the ExportEdgeConfig.ps1 script to the root folder of your user profile on the source server.

2. To export the source server configuration data to a file on the source server, use the following syntax.

   ```powershell
   ./ExportEdgeConfig.ps1 -CloneConfigData:"<configuration file>"
   ```

   For example, to export the source server configuration data to the file C:\\CloneConfigData.xml, run the following command.

   ```powershell
   ./ExportEdgeConfig.ps1 -CloneConfigData:"C:\CloneConfigData.xml"
   ```

## How do you know this step worked?

You'll know you successfully exported the source configuration data to a file when the confirmation message, "Edge configuration data is exported successfully to: \<output file path\>" appears.

## Step 2: Validate the configuration file and create an answer file on the target server

1. Copy the source server configuration file you exported in the previous step to the target Edge Transport server.

2. Copy the ImportEdgeConfig.ps1 script to the root folder of your user profile on the target server.

3. To validate the configuration file and use the results to create an answer file on the target server, use the following syntax.

   ```powershell
   ./ImportEdgeConfig.ps1 -CloneConfigData:"<configuration file>" -IsImport $false -CloneConfigAnswer:"<answer file>"
   ```

   For example, to validate the configuration file C:\\CloneConfigData.xml, and create the answer file C:\\CloneConfigAnswer.xml, run the following command.

   ```powershell
   ./ImportEdgeConfig.ps1 -CloneConfigData:"C:\CloneConfigData.xml" -IsImport $false -CloneConfigAnswer:"C:\CloneConfigAnswer.xml"
   ```

4. Open the answer file and modify any settings that are invalid for the target server. If no modifications are required, the answer file will have no entries. Save your changes.

## How do you know this step worked?

You'll know you successfully validated the configuration file and created an answer file when the confirmation message, "Answer file is successfully created" appears.

## Step 3 Import the configuration file on the target server

To import the configuration file on the target server, use the following syntax.

```powershell
./ImportEdgeConfig.ps1 -CloneConfigData:"<Configuration file>" -IsImport $true -CloneConfigAnswer:"<answer file>"
```

For example, to import the configuration file C:\\CloneConfigData.xml by using the answer file C:\\CloneConfigAnswer.xml, run the following command.

```powershell
./ImportEdgeConfig.ps1 -CloneConfigData:"C:\CloneConfigData.xml" -IsImport $true -CloneConfigAnswer:"C:\CloneConfigAnswer.xml"
```

## How do you know this step worked?

You'll know you successfully imported the configuration file on the target server when the confirmation message, "Importing Edge configuration information succeeded" appears.
