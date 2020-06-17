---
localization_priority: Normal
description: 'Summary: Learn how to configure message size limits for Exchange ActiveSync, Exchange Web Services, and Outlook on the web clients in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: fef9ca78-b68f-4342-ada0-881ab985ce3c
ms.reviewer: 
title: Configure client-specific message size limits
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure client-specific message size limits

In Exchange Server, there are several different message size limits that apply to messages as they travel through your organization. For more information, see [Message size and recipient limits in Exchange Server](../../mail-flow/message-size-limits.md).

However, there are client-specific message size limits you can configure for Outlook on the web (fornerly known as Outlook Web App) and email clients that use Exchange ActiveSync or Exchange Web Services (EWS). If you change the Exchange organizational, connector, or user message size limits, you likely need change the limits for Outlook on the web, ActiveSync, and EWS. These limits are described in the following tables. To change the message size limit for a specific client type, you need to change **all** the values that are described in the table.

> [!NOTE]
> For any message size limit, you need to set a value that's larger than the actual size you want enforced. This accounts for the Base64 encoding of attachments and other binary data. Base64 encoding increases the size of the message by approximately 33%, so the value you specify should be approximately 33% larger than the actual message size you want enforced. For example, if you specify a maximum message size value of 64 MB, you can expect a realistic maximum message size of approximately 48 MB.

## ActiveSync

|**Services**|**Configuration file**|**Keys and default values**|**Size**|
|:-----|:-----|:-----|:-----|
|Client Access (frontend)|`%ExchangeInstallPath%FrontEnd\HttpProxy\Sync\web.config`|`maxAllowedContentLength="30000000"` (not present by default; see comments)|bytes|
|Client Access (frontend)|`%ExchangeInstallPath%FrontEnd\HttpProxy\Sync\web.config`|`maxRequestLength="10240"`|kilobytes|
|Backend|`%ExchangeInstallPath%ClientAccess\Sync\web.config`|`maxAllowedContentLength="30000000 bytes"` (not present by default; see comments)|bytes|
|Backend|`%ExchangeInstallPath%ClientAccess\Sync\web.config`|`maxRequestLength="10240"`|kilobytes|
|Backend|`%ExchangeInstallPath%ClientAccess\Sync\web.config`|`<add key="MaxDocumentDataSize" value="10240000">`|bytes|

### Comments on ActiveSync limits

By default, there is no _maxAllowedContentLength_ key in the `web.config` files for ActiveSync. However, the maximum message size for ActiveSync is affected by the **maxAllowedContentLength** value that is applied to all web sites on the server. The default value is 30000000 bytes. To see these values for ActiveSync on Mailbox servers in IIS Manager, perform the following steps:

1. Do one of the following steps:

   - For the Client Access (frontend) web site, open **IIS Manager**, navigate to **Sites** \> **Default Web Site** and select **Microsoft-Server-ActiveSync**.

   - For the backend web site, open **IIS Manager**, navigate to **Sites** \> **Exchange Back End** and select **Microsoft-Server-ActiveSync**.

2. Verify the **Features View** tab is selected at the bottom, and double-click **Configuration Editor** in the **Management** section.

3. Click the drop down arrow in the **Section** field, navigate to **system.webServer** \> **security** and select **requestFiltering**.

4. In the results, expand **requestLimits**, and you'll see **maxAllowedContentLength** and the default value 30000000 (bytes).

To change the **maxAllowedContentLength** value, enter a new value in bytes, and click **Apply**. You need to change the value on the Client Access web site and the back end web site.

**Note**: You can change the same setting in IIS manager at **Sites** \> **Default Web Site** \> **Microsoft-Server-ActiveSync** or **Sites** \> **Exchange Back End** \> **Microsoft-Server-ActiveSync** and then **Request Filtering** in the **IIS** section \> **Edit Feature Settings** in the **Actions** area \> **Maximum allowed content length (Bytes)** in the **Request Limits** section.

After you change the value in IIS Manager, a new _maxAllowedContentLength_ key is written to the corresponding Client Access or backend web.config file that's described in the table.

## Exchange Web Services

|**Service**|**Configuration file**|**Keys and default values**|**Size**|
|:-----|:-----|:-----|:-----|
|Client Access (frontend)|`%ExchangeInstallPath%FrontEnd\HttpProxy\ews\web.config`|`maxAllowedContentLength="67108864"`|bytes|
|Backend|`%ExchangeInstallPath%ClientAccess\exchweb\ews\web.config`|`maxAllowedContentLength="67108864"`|bytes|
|Backend|`%ExchangeInstallPath%ClientAccess\exchweb\ews\web.config`|14 instances of `maxReceivedMessageSize="67108864"` (for different combinations of http/https bindings and authentication methods)|bytes|

### Comments on EWS limits

- In the backend `web.config` file, there are two instances of the value `maxReceivedMessageSize="1048576"` for **UMLegacyMessageEncoderSoap11Element** bindings that you don't need to modify.

- _maxRequestLength_ is an ASP.NET setting that's present in both web.config files, but isn't used by EWS, so you don't need to modify it.

## Outlook on the web

|**Service**|**Configuration file**|**Keys and default values**|**Size**|
|:-----|:-----|:-----|:-----|
|Client Access (frontend)|`%ExchangeInstallPath%FrontEnd\HttpProxy\owa\web.config`|`maxAllowedContentLength="35000000"`|bytes|
|Client Access (frontend)|`%ExchangeInstallPath%FrontEnd\HttpProxy\owa\web.config`|`maxRequestLength="35000"`|kilobytes|
|Backend|`%ExchangeInstallPath%ClientAccess\Owa\web.config`|`maxAllowedContentLength="35000000"`|bytes|
|Backend|`%ExchangeInstallPath%ClientAccess\Owa\web.config`|`maxRequestLength="35000"`|kilobytes|
|Backend|`%ExchangeInstallPath%ClientAccess\Owa\web.config`|2 instances of `maxReceivedMessageSize="35000000"` (for http and https bindings)|bytes|
|Backend|`%ExchangeInstallPath%ClientAccess\Owa\web.config`|2 instances of `maxStringContentLength="35000000"` (for http and https bindings)|bytes|

### Comments on Outlook on the web limits

- In the backend `web.config` file, there's an instance of the value `maxStringContentLength="102400"` for the **MsOnlineShellService** binding that you don't need to modify.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- Exchange permissions don't apply to the procedures in this topic. These procedures are performed in the operating system of the Exchange server.

- Changes you save to the web.config configuration file are applied after you restart IIS.

- To allow for the 33% increase in size due to Base64 encoding, multiply your desired new maximum size value in megabytes by 4/3. To convert the value into kilobytes, multiply by 1024. To convert the value into bytes, multiply by 1048756 (1024\*1024). Note that the size increase caused by Base64 encoding could be greater than 33%, and depends on several factors (for example, the attachment size, file type, compression, and the email client).

- Any customized Exchange or Internet Information Server (IIS) settings that you made in Exchange XML application configuration files on the Exchange server (for example, web.config files or the EdgeTransport.exe.config file) **will be overwritten** when you install an Exchange CU. Be sure save this information so you can easily re-apply the settings after the install. After you install the Exchange CU, you need to re-configure these settings.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use Notepad to configure a client-specific message size limit

1. Open the appropriate web.config files in Notepad. For example, to open the web.config files for EWS clients, run the following commands:

   ```console
   Notepad %ExchangeInstallPath%ClientAccess\exchweb\ews\web.config
   ```

   ```console
   Notepad %ExchangeInstallPath%FrontEnd\HttpProxy\ews\web.config
   ```

2. Find the relevant keys in the appropriate web.config files as described in the tables earlier in the topic. For example, for EWS clients, find the _maxAllowedContentLength_ key in the Client Access and backend web.config files and all 14 instances of the value `maxReceivedMessageSize="67108864"` in the backend web.config file.

   ```
   <requestLimits maxAllowedContentLength="67108864" />
   ...maxReceivedMessageSize="67108864"...
   ```

   For example, to allow a Base64 encoded maximum message size of approximately 64 MB, change all instances of `67108864` to `89478486` (64\*4/3\*1048756):

   ```
   <requestLimits maxAllowedContentLength="89478486" />
   ...maxReceivedMessageSize="89478486"...
   ```

3. When you're finished, save and close the web.config files.

4. Restart IIS on the Exchange server by using either of the following methods:

   - Open IIS Manager, select the server, and in the **Actions** pane, click **Restart**.

     ![In IIS Manager, select the server, and in the Actions pane, click Restart](../../media/7d37436a-b89d-4010-bef4-f4276686d5ad.png)

   - Run the following commands from an elevated command prompt (a Command Prompt window you open by selecting **Run as administrator**):

     ```console
     net stop was /y
     ```

     ```console
     net start w3svc
     ```

## Configure client-specific message size limits from the command line

Instead of using Notepad, you can also configure the client-specific message size limits from the command line. Open an elevated command prompt on the Exchange server (a Command Prompt window you open by selecting **Run as administrator**) and run the appropriate commands for the limits that you want to configure.

> [!NOTE]
> 
> - The size values in the commands are the default values, so you'll need to change them.
> 
> - Pay attention to whether the value is in bytes or kilobytes.

### ActiveSync

```
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/Microsoft-Server-ActiveSync/" -section:system.webServer/security/requestFiltering /requestLimits.maxAllowedContentLength:30000000
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/Microsoft-Server-ActiveSync/" -section:system.web/httpRuntime /maxRequestLength:10240
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/Microsoft-Server-ActiveSync/" -section:system.webServer/security/requestFiltering /requestLimits.maxAllowedContentLength:30000000
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/Microsoft-Server-ActiveSync/" -section:system.web/httpRuntime /maxRequestLength:10240
%windir%\system32\inetsrv\appcmd.exe set config "Exchange Back End/Microsoft-Server-ActiveSync/" -section:appSettings /[key='MaxDocumentDataSize'].value:10240000
```

### Exchange Web Services

```
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

### Outlook on the web

```
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

To verify that you have successfully configured the client-specific message size limit, you need to send a test message to and from a mailbox by using the affected client. You can try a few smaller attachments or one large attachment so the test messages are approximately 33% less than the value you configured. For example, a configured value of 85 MB results in a realistic maximum message size of approximately 64 MB.
