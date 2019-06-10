---
title: 'Enable support for legacy transport agents: Exchange 2013 Help'
TOCTitle: Enable support for legacy transport agents
ms:assetid: 00617e87-7199-406e-b4a3-94378f657f1f
ms:mtpsurl: https://technet.microsoft.com/en-us/library/JJ591524(v=EXCHG.150)
ms:contentKeyID: 49084757
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Enable support for legacy transport agents

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, transport agents that were created using the Microsoft .NET Framework version 4.0 are supported by default. Exchange 2013 supports transport agents that were created using previous versions of the .NET Framework, but support for these legacy transport agents isn't enabled by default. To enable support for legacy transport agents, you need to modify the appropriate XML application configuration file. The files you need to modify depend on where the transport agent is installed:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Server</th>
<th>Application configuration files</th>
<th>Microsoft Windows service</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Client Access server</p></td>
<td><p>%ExchangeInstallPath%Bin\MSExchangeFrontendTransport.exe.config</p></td>
<td><p>Microsoft Exchange Front End Transport (MSExchangeFrontendTransport)</p></td>
</tr>
<tr class="even">
<td><p>Mailbox server</p></td>
<td><ul>
<li><p>%ExchangeInstallPath%Bin\EdgeTransport.exe.config</p></li>
<li><p>%ExchangeInstallPath%Bin\MSExchangeTransport.exe.config</p></li>
</ul></td>
<td><p>Microsoft Exchange Transport (MSExchangeTransport)</p></td>
</tr>
</tbody>
</table>

Support for legacy transport agents is controlled by keys in the application configuration files. By default, none of the required keys are present in the application configuration files. You must add the keys manually. The following table explains each key in more detail.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Key</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>useLegacyV2RuntimeActivationPolicy</em></p></td>
<td><p>This key enables or disables support for legacy transport agents. Valid values for this key are <code>true</code> or <code>false</code>. If this key isn't specified, the default value is <code>false</code>.</p></td>
</tr>
<tr class="even">
<td><p><em>supportedRuntime version</em></p></td>
<td><p>This key specifies the version of the Microsoft .NET Framework that's required by the agent. Valid values for this key are:</p>
<ul>
<li><p><code>v4.0</code> or <code>v4.0.30319</code></p></li>
<li><p><code>v3.5</code> or <code>v3.5.21022</code></p></li>
<li><p><code>v3.0</code> or <code>v3.0.4506</code></p></li>
<li><p><code>v2.0</code> or <code>v2.0.50727</code></p></li>
</ul>
<p>You specify multiple values using multiple separate instances of the <em>supportedRuntime version</em> key.</p>
<p>When you enable legacy transport agent support using the <em>useLegacyV2RuntimeActivationPolicy</em> key, you should always specify the value <code>v4.0</code> in addition to the values required by the legacy transport agent.</p></td>
</tr>
</tbody>
</table>

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- Exchange permissions don't apply to the procedures in this topic. These procedures are performed in the operating system of the Exchange Server.

- Changes you save to an application configuration file are applied after you restart the corresponding service.

- When you restart any of the services that are associated with the application configuration files, mail flow on the server is temporarily interrupted.

- Any customized per-server settings you make in Exchange XML application configuration files, for example, web.config files on Client Access servers or the EdgeTransport.exe.config file on Mailbox servers, will be overwritten when you install an Exchange Cumulative Update (CU). Make sure that you save this information so you can easily re-configure your server after the install. You must re-configure these settings after you install an Exchange CU.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Command Prompt to configure support for legacy transport agents

Use the following procedure to enable support for legacy transport agents:

1. In a Command prompt window, on the Exchange 2013 server where you want to configure the legacy transport agent support, open the appropriate application configuration file in Notepad by running the following command:

    ```powershell
    Notepad %ExchangeInstallPath%Bin\<AppConfigFile>
    ```

    For example, to open the EdgeTransport.exe.config file on a Mailbox server, run the following command:

    ```powershell
    Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config
    ```

2. Locate the *\</configuration\>* key at the end of the file, and paste the following keys before the *\</configuration\>* key:

    ```powershell
    <startup useLegacyV2RuntimeActivationPolicy="true">
       <supportedRuntime version="v4.0" />
       <supportedRuntime version="v3.5" />
       <supportedRuntime version="v3.0" />
       <supportedRuntime version="v2.0" />
    </startup>
    ```

3. When you are finished, save and close the application configuration file.

4. Repeat Steps 1 through 3 to modify the other application configuration files.

5. Restart the associated Windows service by running the following command:

    ```powershell
    net stop <service> && net start <service>
    ```

    For example, if you modified the EdgeTransport.exe.config file, you need to restart the Microsoft Exchange Transport service by running the following command:

    ```powershell
    net stop MSExchangeTransport && net start MSExchangeTransport
    ```

6. Repeat Step 5 to restart services associated with the other modified application configuration files.

## How do you know this worked?

You'll know this procedure works if the legacy transport agent installs successfully. If you try to install a legacy transport agent without performing the procedures in this topic, you'll receive an error that's similar to the following:

`Mixed mode assembly is built against version '<version>' of the runtime and cannot be loaded in the 4.0 runtime without additional configuration information.`
