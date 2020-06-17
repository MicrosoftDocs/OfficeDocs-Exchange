---
title: 'Download engine and definition updates: Exchange 2013 Help'
TOCTitle: Download engine and definition updates
ms:assetid: 8f2ca383-e463-4df0-aa5d-29afe2f81aaf
ms:mtpsurl: https://technet.microsoft.com/library/JJ657471(v=EXCHG.150)
ms:contentKeyID: 49289349
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Download engine and definition updates

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 administrators can manually download anti-malware engine and definition (signature) updates. We strongly recommend that you download engine and definition updates on your Exchange server prior to placing it in production.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You can only use the Shell to perform this procedure. To learn how to open the Shell in your on-premises Exchange organization, see [Open the Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- To download updates, your computer must be able to access the Internet and be able to establish a connection on TCP port 80 (HTTP). If your organization uses a proxy server for Internet access, see the Use the Shell to configure proxy server settings for anti-malware updates section in this topic.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Anti-malware" entry in the [Anti-spam and anti-malware permissions](anti-spam-and-anti-malware-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to manually download engine and definition updates

To download engine and definition updates, run the following command:

```powershell
& $env:ExchangeInstallPath\Scripts\Update-MalwareFilteringServer.ps1 -Identity <FQDN of server>
```

This example manually downloads the engine and definition updates on the Exchange server named mailbox01.contoso.com:

```powershell
& $env:ExchangeInstallPath\Scripts\Update-MalwareFilteringServer.ps1 -Identity mailbox01.contoso.com
```

Optionally, you can use the *EngineUpdatePath* parameter to download updates from somewhere other than the default location of `http://forefrontdl.microsoft.com/server/scanengineupdate`. You can use this parameter to specify an alternate HTTP address or a UNC path. If you specify a UNC path, the network service must have access to the path.

This example manually downloads engine and definition updates on the Exchange server named mailbox01.contoso.com from the UNC path `\\FileServer01\Data\MalwareUpdates`:

```powershell
& $env:ExchangeInstallPath\Scripts\Update-MalwareFilteringServer.ps1 -Identity mailbox01.contoso.com -EngineUpdatePath \\FileServer01\Data\MalwareUpdates
```

## How do you know this worked?

In order to verify that updates were downloaded successfully, you need to access Event Viewer and view the event log. We recommend that you filter only FIPFS events, as described in the following procedure.

1. From the **Start** menu, click **All Programs** \> **Administrative Tools** \> **Event Viewer**.

2. In Event Viewer, expand the **Windows Logs** folder, and then click **Application**.

3. In the **Actions** menu, click **Filter Current Log**.

4. In the **Filter Current Log** dialog box, from the **Event sources** drop-down list, select the **FIPFS** check box, and then click **OK**.

If engine updates were downloaded successfully, you will see Event ID 6033, which will appear similar to the following:

`MS Filtering Engine Update process performed a successful scan engine update.`

`Scan Engine: Microsoft`

`Update Path: http://forefrontdl.microsoft.com/server/scanengineupdate`

`Last Update time: ‎2012‎-‎08‎-‎16T13:22:17.000Z`

`Engine Version: 1.1.8601.0`

`Signature Version: 1.131.2169.0`

## Use the Shell to configure proxy server settings for anti-malware updates

If your organization uses a proxy server to control access to the Internet, you need to identify the proxy server so anti-malware engine and definition updates can be downloaded successfully. Proxy server settings that are available using the **Netsh.exe** tool, Internet Explorer connection settings, and the *InternetWebProxy* parameter on the **Set-ExchangeServer** cmdlet don't affect how anti-malware updates are downloaded.

To configure the proxy server settings for anti-malware updates, perform the following steps.

1. Run the following command:

    ```powershell
    Add-PsSnapin Microsoft.Forefront.Filtering.Management.Powershell
    ```

2. Use the **Get-ProxySettings** and **Set-ProxySettings** cmdlets to view and configure the proxy server settings that are used to download anti-malware updates. The **Set-ProxySettings** cmdlet uses the following syntax:

    ```powershell
    Set-ProxySettings -Enabled <$true | $false> -Server <Name or IP address of proxy server> -Port <TCP port of proxy server>
    ```

    For example, to configure anti-malware updates to use the proxy server at address 172.17.17.10 on TCP port 80, run the following command.

    ```powershell
    Set-ProxySettings -Enabled $true -Server 172.17.17.10 -Port 80
    ```

    To verify the proxy server settings, run the **Get-ProxySettings** cmdlet.

## For more information

[Configure anti-malware policies](configure-anti-malware-policies-exchange-2013-help.md)
