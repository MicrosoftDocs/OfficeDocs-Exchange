---
title: 'Configure client-specific message size limits: Exchange 2013 Help'
TOCTitle: Configure client-specific message size limits
ms:assetid: fef9ca78-b68f-4342-ada0-881ab985ce3c
ms:mtpsurl: https://technet.microsoft.com/library/Hh529949(v=EXCHG.150)
ms:contentKeyID: 50934227
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure client-specific message size limits

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, there are several different message size limits that apply to messages as they travel through your Exchange organization. For more information, see [Message size limits](message-size-limits-exchange-2013-help.md).

However, there are client-specific message size limits you can configure for Outlook Web App and email clients that use ActiveSync or Exchange Web Services (EWS). If you change the Exchange organization-wide message size limits, you need to verify that the message size limits for Outlook Web App, ActiveSync, and Exchange Web Services are set accordingly. You configure these values in web.config files on Client Access servers and Mailbox servers. These limits are described in the following tables.

### ActiveSync

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Server role</th>
<th>Configuration file</th>
<th>Keys and default values</th>
<th>Size</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Client Access</p></td>
<td><p><code>%ExchangeInstallPath%FrontEnd\HttpProxy\Sync\web.config</code></p></td>
<td><p><code>maxAllowedContentLength=&quot;30000000 bytes&quot;</code>   Not present by default (see comments).</p></td>
<td><p>bytes</p></td>
</tr>
<tr class="even">
<td><p>Client Access</p></td>
<td><p><code>%ExchangeInstallPath%FrontEnd\HttpProxy\Sync\web.config</code></p></td>
<td><p><code>maxRequestLength=&quot;10240&quot;</code></p></td>
<td><p>kilobytes</p></td>
</tr>
<tr class="odd">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\Sync\web.config</code></p></td>
<td><p><code>maxAllowedContentLength=&quot;30000000 bytes&quot;</code>   Not present by default (see comments).</p></td>
<td><p>bytes</p></td>
</tr>
<tr class="even">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\Sync\web.config</code></p></td>
<td><p><code>maxRequestLength=&quot;10240&quot;</code></p></td>
<td><p>kilobytes</p></td>
</tr>
<tr class="odd">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\Sync\web.config</code></p></td>
<td><p><code>&lt;add key=&quot;MaxDocumentDataSize&quot; value=&quot;10240000&quot;&gt;</code></p></td>
<td><p>bytes</p></td>
</tr>
</tbody>
</table>

#### Comments on ActiveSync limits

By default, there is no *maxAllowedContentLength* key in the `web.config` files for ActiveSync. However, the maximum message size for ActiveSync is affected by the **maxAllowedContentLength** value that is applied to all web sites on the server. The default value is 30000000 bytes (30 MB). To see these values for ActiveSync on Client Access Servers and Mailbox servers in IIS Manager, perform the following steps:

1. Do one of the following steps:

   - On Client Access servers, open **IIS Manager**, navigate to **Sites** \> **Default Web Site** and select **Microsoft-Server-ActiveSync**.

   - On Mailbox servers, open **IIS Manager**, navigate to **Sites** \> **Exchange Back End** and select **Microsoft-Server-ActiveSync**.

2. Verify **Features View** is selected, and double-click **Configuration Editor** in the **Management** section.

3. Click the dropdown arrow in the **Section** field, navigate to **system.webServer** \> **security** and select **requestFiltering**.

4. In the results, expand **requestLimits**, and you'll see **maxAllowedContentLength** and the default value 30000000 (bytes).

To change the **maxAllowedContentLength** value, enter a new value in bytes, and click **Apply**. You need to change the value on Client Access servers and on Mailbox servers. After you change the value in IIS Manager, a new *maxAllowedContentLength* key is written to the corresponding `web.config` file (`%ExchangeInstallPath%FrontEnd\HttpProxy\Sync\web.config` on Client Access servers, and `%ExchangeInstallPath%ClientAccess\Sync\web.config` on Mailbox servers).

To change the maximum message size for ActiveSync clients, you need to change the value of *maxRequestLength* in the `web.config` file on Client Access servers and Mailbox servers, *MaxDocumentDataSize* in the `web.config` file on Mailbox servers, and *maxAllowedContentLength* in IIS Manager on Client Access servers and Mailbox servers.

### Exchange Web Services

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Serve role</th>
<th>Configuration file</th>
<th>Keys and default values</th>
<th>Size</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Client Access</p></td>
<td><p><code>%ExchangeInstallPath%FrontEnd\HttpProxy\ews\web.config</code></p></td>
<td><p><code>maxAllowedContentLength=&quot;67108864&quot;</code></p></td>
<td><p>bytes</p></td>
</tr>
<tr class="even">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\exchweb\ews\web.config</code></p></td>
<td><p><code>maxAllowedContentLength=&quot;67108864&quot;</code></p></td>
<td><p>bytes</p></td>
</tr>
<tr class="odd">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\exchweb\ews\web.config</code></p></td>
<td><p>14 instances of <code>maxReceivedMessageSize=&quot;67108864&quot;</code></p></td>
<td><p>bytes</p></td>
</tr>
</tbody>
</table>

#### Comments on Exchange Web Services limits

- There are 14 separate instances of the value `maxReceivedMessageSize="67108864"` that correspond to different combinations of bindings (http and https) and authentication methods.

- To change the maximum message size for Exchange Web Services clients, you need to change the value of *maxAllowedContentLength* in both `web.config` files, and all 14 instances of `maxReceivedMessageSize="67108864"` in the `web.config` file on Mailbox servers.

- In the `web.config` file on Mailbox servers, there are also two instances of the value `maxReceivedMessageSize="1048576"` for **UMLegacyMessageEncoderSoap11Element** bindings that you don't need to modify.

- *maxRequestLength* is an ASP.NET setting that's present in both web.config files, but is not used by Exchange Web Services, so you don't need to modify it.

### Outlook Web App

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Server role</th>
<th>Configuration file</th>
<th>Keys and default values</th>
<th>Size</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Client Access</p></td>
<td><p><code>%ExchangeInstallPath%FrontEnd\HttpProxy\owa\web.config</code></p></td>
<td><p><code>maxAllowedContentLength=&quot;35000000&quot;</code></p></td>
<td><p>bytes</p></td>
</tr>
<tr class="even">
<td><p>Client Access</p></td>
<td><p><code>%ExchangeInstallPath%FrontEnd\HttpProxy\owa\web.config</code></p></td>
<td><p><code>maxRequestLength=&quot;35000&quot;</code></p></td>
<td><p>kilobytes</p></td>
</tr>
<tr class="odd">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\Owa\web.config</code></p></td>
<td><p><code>maxAllowedContentLength=&quot;35000000&quot;</code></p></td>
<td><p>bytes</p></td>
</tr>
<tr class="even">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\Owa\web.config</code></p></td>
<td><p><code>maxRequestLength=&quot;35000&quot;</code></p></td>
<td><p>kilobytes</p></td>
</tr>
<tr class="odd">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\Owa\web.config</code></p></td>
<td><p>2 instances of <code>maxReceivedMessageSize=&quot;35000000&quot;</code></p></td>
<td><p>bytes</p></td>
</tr>
<tr class="even">
<td><p>Mailbox</p></td>
<td><p><code>%ExchangeInstallPath%ClientAccess\Owa\web.config</code></p></td>
<td><p>2 instances of <code>maxStringContentLength=&quot;35000000&quot;</code></p></td>
<td><p>bytes</p></td>
</tr>
</tbody>
</table>

#### Comments on Outlook Web App limits

- In the `web.config` file on Mailbox servers, there are two separate instances of the values `maxReceivedMessageSize="35000000"` and `maxStringContentLength="35000000"` that correspond to http and https bindings.

- To change the maximum message size for Outlook Web App clients, you need to change all of these values in both files, including both instances of *maxReceivedMessageSize* and *maxStringContentLength* in the `web.config` file on Mailbox servers.

- In the `web.config` file on Mailbox servers, there is also an instance of the value `maxStringContentLength="102400"` for the **MsOnlineShellService** binding that you don't need to modify.

For all message size limits, you need to set values that are larger than the actual sizes you want enforced. This increase in values is necessary to account for the inevitable message size increase that occurs after the message attachments and any other binary data are Base64 encoded. Base64 encoding increases the size of the message by approximately 33%, so the values you specify for any message size limits are approximately 33% larger than the actual usable message sizes. For example, if you specify a maximum message size value of 64 MB, you can expect a realistic maximum message size value of approximately 48 MB.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- Exchange permissions don't apply to the procedures in this topic. These procedures are performed in the operating system of the Exchange Server.

- Changes you save to the Web.config configuration file are applied after you restart IIS.

- To allow for a 33% increase in size due to Base64 encoding, multiply your desired new maximum size value in megabytes by 4/3. To convert the value into kilobytes, multiply by 1024. To convert the value into bytes, multiply by 1048576 (1024\*1024). Note that the size increase caused by Base64 encoding could be greater than 33%, and depends on several factors, for example, the attachment file size, type, compression, and the email client used to compose and send the message.

- Any customized per-server settings you make in Exchange XML application configuration files, for example, web.config files on Client Access servers or the EdgeTransport.exe.config file on Mailbox servers, will be overwritten when you install an Exchange Cumulative Update (CU). Make sure that you save this information so you can easily re-configure your server after the install. You must re-configure these settings after you install an Exchange CU.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use Notepad to configure a client-specific message size limit

1. Open the appropriate web.config files in Notepad. For example, to open the web.config files for Exchange Web Services clients, run the following commands:

   ```powershell
   Notepad %ExchangeInstallPath%ClientAccess\exchweb\ews\web.config
   Notepad %ExchangeInstallPath%FrontEnd\HttpProxy\ews\web.config
   ```

2. Find the relevant keys in the appropriate web.config files as described in the tables earlier in the topic. For example, for Exchange Web Services clients, find the *maxAllowedContentLength* key in both files and all 14 instances of the value `maxReceivedMessageSize="67108864"` in the `web.config` file on Mailbox servers.

   ```powershell
   <requestLimits maxAllowedContentLength="67108864" />
   ...maxReceivedMessageSize="67108864"...
   ```

   For example, to allow a Base64 encoded maximum message size of approximately 64 MB, change all instances of `67108864` to `89478486` (64\*4/3\*1048576):

   ```powershell
   <requestLimits maxAllowedContentLength="89478486" />
   ...maxReceivedMessageSize="89478486"...
   ```

3. When you are finished, save and close the web.config files.

4. Restart IIS by running the following command:

   ```powershell
   IISReset /noforce
   ```

## Configure client-specific message size limits from the command line

Instead of using Notepad, you can also configure the client-specific message size limits from the command line. Open an elevated command prompt on the Exchange server (a Command Prompt window you open by selecting **Run as administrator**) and run the appropriate commands for the limits that you want to configure.

**Notes**:

- The size values in the commands are the default values, so you'll need to change them.

- Pay attention to whether the value is in bytes or kilobytes.

### ActiveSync

```powershell
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/Microsoft-Server-ActiveSync/" -section:system.webServer/security/requestFiltering /requestLimits.maxAllowedContentLength:30000000
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/Microsoft-Server-ActiveSync/" -section:system.web/httpRuntime /maxRequestLength:10240
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/Microsoft-Server-ActiveSync/" -section:system.webServer/security/requestFiltering /requestLimits.maxAllowedContentLength:30000000
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/Microsoft-Server-ActiveSync/" -section:system.web/httpRuntime /maxRequestLength:10240
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/Microsoft-Server-ActiveSync/" -section:appSettings /[key='MaxDocumentDataSize'].value:10240000
```

### Exchange Web Services

```powershell
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/ews/" -section:system.webServer/security/requestFiltering /requestLimits.maxAllowedContentLength:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.webServer/security/requestFiltering /requestLimits.maxAllowedContentLength:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSAnonymousHttpsBinding'].httpsTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSAnonymousHttpBinding'].httpTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSBasicHttpsBinding'].httpsTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSBasicHttpBinding'].httpTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSNegotiateHttpsBinding'].httpsTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSNegotiateHttpBinding'].httpTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSWSSecurityHttpsBinding'].httpsTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSWSSecurityHttpBinding'].httpTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSWSSecuritySymmetricKeyHttpsBinding'].httpsTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSWSSecuritySymmetricKeyHttpBinding'].httpTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSWSSecurityX509CertHttpsBinding'].httpsTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /customBinding.[name='EWSWSSecurityX509CertHttpBinding'].httpTransport.maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /webHttpBinding.[name='EWSStreamingNegotiateHttpsBinding'].maxReceivedMessageSize:67108864
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/ews/" -section:system.serviceModel/bindings /webHttpBinding.[name='EWSStreamingNegotiateHttpBinding'].maxReceivedMessageSize:67108864
```

### Outlook Web App

```powershell
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/owa/" -section:system.webServer/security/requestFiltering /requestLimits.maxAllowedContentLength:35000000
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/owa/" -section:system.web/httpRuntime /maxRequestLength:35000
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/owa/" -section:system.webServer/security/requestFiltering /requestLimits.maxAllowedContentLength:35000000
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/owa/" -section:system.web/httpRuntime /maxRequestLength:35000
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/owa/" -section:system.serviceModel/bindings /webHttpBinding.[name='httpsBinding'].maxReceivedMessageSize:35000000
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/owa/" -section:system.serviceModel/bindings /webHttpBinding.[name='httpBinding'].maxReceivedMessageSize:35000000
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/owa/" -section:system.serviceModel/bindings /webHttpBinding.[name='httpsBinding'].readerQuotas.maxStringContentLength:35000000
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/owa/" -section:system.serviceModel/bindings /webHttpBinding.[name='httpBinding'].readerQuotas.maxStringContentLength:35000000
```

## How do you know this worked?

To verify that you have successfully configured the client-specific message size limit, you need to send a test message to and from a mailbox that's being accessed by the affected client. You can try a few smaller attachments or one large attachment so the test messages are approximately 33% less than the value you configured. For example, a configured value of 85 MB results in a realistic maximum message size of approximately 64 MB.
