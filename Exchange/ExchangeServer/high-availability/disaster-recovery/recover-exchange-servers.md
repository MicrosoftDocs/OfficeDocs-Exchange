---
localization_priority: Normal
description: 'Summary: Learn how to recover a lost Exchange 2016 or Exchange 2019 server.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 46e9a1cf-b64c-43c3-a898-6171176da761
ms.reviewer:
title: Recover Exchange server, recover lost Exchange Server, Lost Exchange Server recovery
ms.collection:
- Strat_EX_Admin
- exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Recover Exchange servers

You can recover a lost Exchange server by using the _/Mode:RecoverServer_ switch in unattended mode (from the command line) of Exchange Setup. Since most Exchange server settings are stored in Active Directory, the `Setup.exe /Mode:RecoverServer` command uses that information during the installation of Exchange on a new server with the same name.

Recovering a lost Exchange server is often accomplished by using new hardware. However, you can also use an existing server that doesn't already have Exchange installed on it.

This topic shows you how to recover a lost Exchange server that isn't a member of a database availability group (DAG). For detailed steps about how to recover a server that was a member of a DAG, see [Recover a database availability group member server](recover-dag-member-servers.md).

Looking for other management tasks related to backing up and restoring data? Check out [Backup, restore, and disaster recovery](disaster-recovery.md).

## What do you need to know before you begin?

- Estimated time to complete: 20 minutes

- The account that you'll use to do the server recovery requires the following permissions:

   - Domain Admins security group membership.

   - Exchange Organization Management role group membership.

- If Exchange is installed in a location other than the default location of %ProgramFiles%\Microsoft\Exchange Server\V15, you must include the _/TargetDir:\<Path\>_ switch in the `Setup.exe /Mode:RecoverServer` command to specify the location of the Exchange program (binary) files. If you don't use the _/TargetDir_ switch, the Exchange files will be installed in the default location when you recover the Exchange server.

   To find the install location of Exchange on the lost Exchange server, do the following steps:

   1. Open ADSIEDIT.MSC or LDP.EXE.

   2. Go to **CN=ExServerName,CN=Servers,CN=First Administrative Group,CN=Administrative Groups,CN=ExOrg Name,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=DomainName,CN=Com**

   3. Right-click the Exchange server object, and then click **Properties**.

   4. Find the **msExchInstallPath** attribute. This attribute stores the current installation path.

- You can recover a server using the latest available Cumulative Update (CU), in case you do not have the installable media for CU that was installed on the server to be recovered. Only the last two CUs are available for download. For more information, see [Updates for Exchange Server](../../new-features/updates.md).

- The target server must use the same version of Windows Server as the lost server. For example, you can't recover a lost Exchange 2016 server that was running Windows 2012 R2 on a new server that's running Windows 2016, or vice-versa.

- The same disk drive letters that were used for mounted databases on the lost server must also exist on the target server.

- The target server should have the same general performance characteristics and hardware configuration as the lost server.

- The _/Mode:RecoverServer_ switch assigns a self-signed certificate to all Exchange Services that require SSL/TLS. If the server previously used an SSL/TLS certificate that was issued by a different certification authority, you'll need to re-import the certificate and configure the services to use the certificate. Otherwise, users will get a certificate prompt when they try to connect (for example, in Outlook).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Recover a Lost Exchange Server

1. Reset the computer account for the lost server. For detailed steps, see [Reset a Computer Account](https://go.microsoft.com/fwlink/p/?linkId=165388).

2. Install the proper operating system and name the new server with the same name as the lost server. Recovery won't succeed if the target Windows server doesn't have the same name as the lost Exchange server.

3. Join the server to the same domain as the lost server.

4. Install the necessary prerequisites and operating system components on the target server. For details, see [Exchange Server system requirements](../../plan-and-deploy/prerequisites.md).

5. On the target server, open File Explorer, right-click on the Exchange ISO image file that you downloaded, and then select **Mount**. Note the virtual DVD drive letter that's assigned.

6. Open a Windows Command Prompt window. For example:

    - Press the Windows key + 'R' to open the **Run** dialog, type cmd.exe, and then press **OK**.

    - Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

7. In the Command Prompt window, use the following syntax:

    ```console
    <Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms /Mode:RecoverServer [/TargetDir:<Path>] [/DomainController:<ServerNameOrFQDN>] [/DoNotStartTransport] [/EnableErrorReporting]
    ```

    This example uses the Exchange installation files on drive E: to install Exchange in the default location (%ProgramFiles%\Microsoft\Exchange Server\V15) and recover the Exchange server.

    ```powershell
    E:\Setup.exe /IAcceptExchangeServerLicenseTerms /Mode:RecoverServer
    ```

    This is the same example, but a custom location for the Exchange program files is required to match the location on the lost server.

    ```powershell
    E:\Setup.exe /IAcceptExchangeServerLicenseTerms /Mode:RecoverServer /TargetDir:"D:\Program Files\Exchange"
    ```

    For more information about the optional switches, see [Use unattended mode in Exchange Setup](../../plan-and-deploy/deploy-new-installations/unattended-installs.md).

8. After Setup has completed, but before you put the recovered server into production, reconfigure any custom settings that were previously present on the server, and then restart the server.

## How do you know this worked?

The successful completion of Setup will be the primary indicator that the recovery was successful. To further verify that you've successfully recovered a lost server, open the Windows Services tool (services.msc) and verify that the Microsoft Exchange services have been installed and are running.

### Possible issues with the Scripting Agent

If you previously enabled the Scripting Agent in your Exchange organization, the recovery process might fail. The error will look like this:

```console
"Initialization failed: '"Scripting Agent initialization failed: "File is not found: 'C:\Program Files\Microsoft\Exchange Server\V15\Bin\CmdletExtensionAgents\ScriptingAgentConfig.xml'.""' ---> Microsoft.Exchange.Provisioning.ProvisioningException: "Scripting Agent initialization failed: "File is not found: 'C:\Program Files\Microsoft\Exchange Server\V15\Bin\CmdletExtensionAgents\ScriptingAgentConfig.xml'."" ---> System.IO.FileNotFoundException: "File is not found: 'C:\Program Files\Microsoft\Exchange Server\V15\Bin\CmdletExtensionAgents\ScriptingAgentConfig.xml'."
```

If you have other Exchange servers in your organization, you'll need to:

1. Disable the Scripting Agent in the Exchange Management Shell on an existing server:

    ```powershell
    Disable-CmdletExtensionAgent -Identity "Scripting Agent"
    ```

2. Run Exchange Setup in recovery mode as described earlier in this topic.

3. Enable the Scripting Agent in the Exchange Management Shell after the Exchange server recovery is complete:

    ```powershell
    Enable-CmdletExtensionAgent -Identity "Scripting Agent"
    ```

If the recovered Exchange server is the only Exchange server in your organization, you'll need to:

1. Rename the file %ExchangeInstallPath%Bin\CmdletExtensionAgents\ScriptingAgentConfig.**xml.sample** to %ExchangeInstallPath%Bin\CmdletExtensionAgents\ScriptingAgentConfig.**xml**.

    The default value of %ExchangeInstallationPath% is %ProgramFiles%\Microsoft\Exchange Server\V15\, but the actual value is wherever you installed Exchange on the server.

2. Re-run Exchange Setup in recovery mode as described earlier in this topic.
